# MiniDisc Label Designer

## Concept Document

**Project Codename:** MDLabel Studio
**Version:** 0.4 (Concept)
**Date:** March 10, 2026

---

## 1. Executive Summary

This document outlines the concept for **MDLabel Studio**, a full-stack web application for designing and printing professional-quality MiniDisc labels. The frontend is built with React, TypeScript, and Tailwind CSS v4, providing an interactive visual editor. The backend is built with FastAPI (Python), serving as the authoritative engine for metadata lookup, image processing, color extraction, label rendering, print-ready PDF generation, and **AI-powered creative label design**.

MDLabel Studio operates in two distinct modes. **Album Mode** replicates official album releases with auto-imported metadata, cover art, and tracklists. **Mixtape Mode** is where things get creative: it generates fully original label designs for custom compilations, personal mixes, and curated playlists, powered by a multi-LLM creative engine that can generate artwork concepts, thematic color palettes, custom taglines, and stylized layouts based on natural language descriptions like "Star Citizen theme, tech-inspired, for my lofi beats vol 4."

The full-stack architecture serves four distinct audiences through a single codebase. Human users interact through the React web UI. **Airi** calls the FastAPI endpoints directly as part of automated workflows --- and in Mixtape Mode, Airi can inject her own personality and creative preferences into the label design. **Manus** operates as a full agentic creative partner, handling end-to-end label creation with research, content generation, image generation, and iterative refinement in a single autonomous workflow. The FOSS community can script against the same API using curl, Python requests, or any HTTP client, with no web browser required.

The AI creative engine is built on a **provider-agnostic LLM integration layer** supporting OpenRouter, Claude (Anthropic), OpenAI, Google Gemini, self-hosted vLLM instances, and **Manus as a unified content-and-image provider**. This ensures no vendor lock-in and allows users to route creative generation through whichever model or agent best suits their needs or budget.

---

## 2. Problem Statement

Creating custom MiniDisc insert cards today requires a multi-step manual workflow that is time-consuming and error-prone. The typical process involves opening a PSD template in Affinity Designer (or Photoshop), manually placing album artwork, typing out tracklists, adjusting font sizes to fit, picking colors by hand, aligning elements to the template grid, and exporting at the correct DPI for print. Each new album requires repeating this entire process from scratch.

The existing web-based tools in the MiniDisc community solve only a fraction of this problem. They generate disc-face labels (the small rectangular sticker on the MiniDisc cartridge itself) but do not address the more complex insert card layout, which includes a spine, an information panel, a main artwork area, and a multi-column tracklist section. The insert card is where the real design effort lives, and it is the component most visible when a MiniDisc sits on a shelf.

Beyond official album replication, there is an entirely unaddressed use case: **custom mixtape labels**. The MiniDisc format has always been synonymous with personal mix culture --- the 90s and early 2000s tradition of curating playlists, recording them to disc, and creating custom artwork to match. Today's MiniDisc revival community is doing exactly this, but with no tooling purpose-built for the creative side. People are making mixtapes, lofi compilations, DJ sets, and curated mood playlists on MiniDisc, but the label design process is entirely manual and disconnected from the creative intent behind the mix.

| Current Pain Point | Impact |
|---|---|
| Manual template editing in Affinity/Photoshop | High time cost per label, requires design software skills |
| No tracklist auto-import | Every track must be typed manually, prone to typos |
| Color selection is trial-and-error | Difficult to match album art aesthetic consistently |
| No batch processing | Multi-disc albums require repeating the entire workflow |
| Template geometry is fragile | Easy to accidentally misalign elements or break bleed margins |
| No disc-face label integration | Disc labels and insert cards are designed in separate workflows |
| CJK text handling is manual | Japanese/Korean track titles require careful font and layout management |
| No programmatic access | Cannot automate label creation from scripts or AI assistants |
| **No custom mixtape support** | **No tool helps design original labels for personal mixes, compilations, or curated playlists** |
| **No AI-assisted creativity** | **No tool leverages LLMs or agentic AI for theme generation, artwork concepts, or creative direction** |

---

## 3. Feasibility Assessment

**Verdict: Highly feasible.** Every component of this application can be built with mature, well-supported technologies. The full-stack approach actually simplifies several technical challenges by moving rendering and image processing to the Python backend, where libraries like Pillow, ReportLab, and CairoSVG are more mature and predictable than their JavaScript equivalents. The AI creative engine leverages the same OpenAI-compatible API pattern that OpenRouter, Anthropic, OpenAI, Google, and vLLM all support, making multi-provider integration straightforward. Manus integration adds a uniquely powerful agentic layer that can handle the entire creative pipeline autonomously.

### 3.1 Technical Feasibility

The application requires five core technical capabilities. The full-stack architecture distributes these across the frontend and backend according to where each capability is strongest.

**Precise layout rendering at print dimensions** is handled server-side using Python's ReportLab library [3] or CairoSVG for vector-to-raster conversion. ReportLab generates PDFs with exact millimeter dimensions, embedded fonts, and CMYK color space support natively. This eliminates the browser-dependent rendering inconsistencies that plague client-side PDF generation. The frontend uses Konva.js (via react-konva) for the interactive preview, but the backend is the source of truth for the final exported output.

**Tracklist auto-import** is supported by multiple free APIs, all called from the backend to keep API keys and rate-limiting logic centralized. The MusicBrainz API [4] provides comprehensive album metadata including track titles, durations, and disc numbers, with no authentication required for basic lookups. The Cover Art Archive [5], linked to MusicBrainz, provides album artwork. The Discogs API [6] offers similar metadata with rich catalog data. The Spotify Web API [7] provides album data, track listings, and high-resolution cover art, though it requires OAuth authentication. The backend proxies these lookups, caches results, and normalizes the response format regardless of source.

**Color extraction from images** is handled server-side using Python's colorthief library or the fast-colorthief variant, which analyzes an uploaded album art image and returns a palette of dominant colors. Running this on the backend means the full-resolution image is processed once, and the palette is returned to the frontend as part of the project state. The backend also performs WCAG contrast ratio calculations to ensure text legibility.

**Print-ready PDF generation** with exact physical dimensions, bleed margins, and crop marks is the backend's primary output. ReportLab generates PDFs with millimeter-precision page dimensions, vector crop marks, and embedded high-resolution raster images at 300 DPI. The backend can also generate PNG and SVG outputs using Pillow and CairoSVG respectively. All rendering logic lives in one place, ensuring that the output from the web UI, from Airi, from Manus, and from a FOSS script are identical.

**AI-powered creative generation** is supported through two complementary approaches. The first is the **multi-LLM creative engine**, which integrates with LLM providers through LangChain's chat model abstraction [15] to generate thematic design directions, color palettes, taglines, layout suggestions, and image generation prompts from natural language descriptions. The second is the **Manus agentic integration**, which treats Manus as a unified creative partner capable of handling the entire design pipeline --- from researching visual references, to generating design concepts, to producing artwork, to iterating on feedback --- in a single autonomous workflow. Both approaches are entirely optional; the application is fully functional without any AI configuration.

### 3.2 Comparison with Existing Tools

The following table compares MDLabel Studio's proposed capabilities against the two most prominent existing tools in the MiniDisc community.

| Feature | MD Label Generator [1] | MD Cover Designer [2] | MDLabel Studio (Proposed) |
|---|---|---|---|
| **Architecture** | Client-side only | Client-side (Angular) | **Full-stack: React + FastAPI** |
| **Label Types** | Disc face label only | Disc face + spine | Disc face + spine + **full insert card** + disc label grid |
| **Tracklist Support** | None | Via Spotify API | MusicBrainz, Discogs, Spotify, manual entry |
| **Color Theming** | Dark/Light toggle | Manual color selection | **Auto-extract from album art** + **AI-generated themes** + manual override |
| **CJK Text** | Limited | Unknown | **First-class support** with font selection |
| **Multi-disc Albums** | No | No | **Yes, with disc numbering and color variants** |
| **Custom Mixtape Labels** | No | No | **Yes, with AI-assisted creative design** |
| **AI Art Generation** | No | No | **Yes, via multi-provider LLM + image gen + Manus** |
| **Agentic AI Workflow** | No | No | **Yes, Manus handles end-to-end creative pipeline** |
| **Bleed Margins & Crop Marks** | No | Cut-line toggle | **Full print-ready output with bleed and crop marks** |
| **Batch Disc Labels** | No | No | **Grid layout for batch printing disc face labels** |
| **Export Format** | PNG | PNG | **PDF (print-ready) + PNG + SVG** |
| **Project Save/Load** | No | Yes (JSON) | Yes (JSON + database persistence) |
| **AI Integration** | No | No | **Airi + Manus + FOSS scripting** |
| **Programmatic Access** | No | No | **Full REST API for scripting** |
| **Template System** | Fixed layout | Customizable | **Multiple templates + AI-suggested layouts** |

---

## 4. Product Vision

### 4.1 Two Modes, One Tool

MDLabel Studio operates in two modes that share the same rendering engine, template system, and export pipeline, but differ fundamentally in where the content comes from.

**Album Mode** is the structured workflow for replicating official releases. The user searches for an album, the backend fetches metadata and artwork, and the label is assembled automatically. The creative decisions are minimal: choose a color theme, adjust the layout, and export. This mode is optimized for speed and accuracy --- the goal is a professional-looking label that faithfully represents the original release.

**Mixtape Mode** is the creative workflow for original compositions. There is no album to look up, no official artwork to import, and no canonical tracklist format to follow. Instead, the user describes what they want --- a theme, a mood, a visual reference --- and the AI creative engine generates design suggestions. The user can accept, modify, or reject each suggestion, building the label iteratively through a conversation between human intent and AI creativity. This is the mode that brings back the spirit of hand-crafting mixtape covers in the 90s, but with the power of modern AI to accelerate the creative process.

The two modes share the same underlying data model (`LabelProject`), the same rendering engine, and the same export pipeline. A label created in Mixtape Mode is indistinguishable from one created in Album Mode at the rendering level --- the difference is entirely in how the content was sourced.

### 4.2 Album Mode Workflow

The Album Mode workflow is designed to take a label from concept to print-ready output in under two minutes.

**Step 1 --- Import.** The user enters an album name or searches via the integrated metadata lookup. The frontend sends the query to the FastAPI backend, which searches MusicBrainz (or Discogs/Spotify), fetches the album title, artist name, year, tracklist (with durations), disc count, and cover art, and returns a normalized album data object. Alternatively, the user can manually enter this information or import from a JSON/CSV file.

**Step 2 --- Arrange.** The tracklist is automatically arranged into the optimal column layout based on the number of tracks. Track durations are optionally displayed, right-aligned. The album art is placed in the main artwork area and scaled to fit.

**Step 3 --- Style.** The backend extracts a color palette from the album art and returns suggested background, text, and accent colors. The user can accept the suggestion, choose from preset themes, or manually pick colors.

**Step 4 --- Refine.** The user can adjust any element: reposition artwork, change font sizes, edit text, toggle track durations, add a disc number suffix, choose spine text orientation, and toggle between light and dark text.

**Step 5 --- Export.** The user clicks Export, and the backend generates a print-ready PDF at 300 DPI with proper bleed margins.

### 4.3 Mixtape Mode Workflow

Mixtape Mode introduces a fundamentally different creative flow, powered by the AI creative engine.

**Step 1 --- Describe.** The user provides a creative brief in natural language. This can be as simple as "lofi beats vol 4, Star Citizen theme, techy vibes" or as detailed as a paragraph describing the mood, color preferences, visual references, and typography style. In the web UI, this is a text area with optional structured fields for title, subtitle, artist/DJ name, and theme keywords. Via the API (for Airi, Manus, or scripts), it is a single `creative_brief` string field.

**Step 2 --- Generate.** The backend sends the creative brief to the AI creative engine (or Manus, if selected as the provider), which returns a `MixtapeDesignConcept` --- a structured Pydantic v2 model containing:

| Field | Type | Description |
|---|---|---|
| `title_treatment` | string | Suggested title styling (e.g., "LOFI BEATS" in large caps, "vol. 4" in small italic) |
| `tagline` | string | Optional subtitle or tagline (e.g., "Transmissions from Stanton System") |
| `color_palette` | ColorPalette | Background, text, accent, and secondary colors |
| `typography` | TypographySuggestion | Font family, weight, and size recommendations |
| `artwork_prompt` | string | A detailed image generation prompt (1000+ characters) for the cover art |
| `layout_variant` | string | Suggested template variant (e.g., "minimal-centered", "retro-grid", "cinematic-wide") |
| `mood_tags` | list[str] | Descriptive tags for the overall aesthetic |
| `design_rationale` | string | Brief explanation of why these choices were made |

**Step 3 --- Artwork.** If the user does not provide their own artwork, the backend can generate cover art using the `artwork_prompt` from the design concept. The image generation request is routed through the configured provider (DALL-E via OpenAI, Flux via OpenRouter, Stable Diffusion via API, a self-hosted endpoint, or **Manus for integrated generation**). The user can regenerate with variations, upload their own image, or skip artwork entirely for a typography-only design.

**Step 4 --- Assemble.** The design concept is applied to the label template. The user sees a live preview with the AI-suggested colors, typography, layout, and (if generated) artwork. The tracklist is entered manually or pasted from a text source. From this point forward, the workflow merges with Album Mode --- the user can refine any element and export.

**Step 5 --- Iterate.** The user can ask for variations: "make it darker," "try a more retro feel," "change the accent to neon green." Each request goes back to the AI creative engine as a refinement prompt, and the design concept is updated accordingly. This iterative loop is the heart of Mixtape Mode --- it is a creative conversation, not a one-shot generation.

### 4.4 Label Types Supported

The application supports three distinct label types, each with its own template and geometry. Both Album Mode and Mixtape Mode produce the same label types.

**Insert Card (Primary Focus).** This is the folded card that sits inside the MiniDisc jewel case. Based on analysis of the provided label designs and standard MiniDisc case dimensions [8] [9], the insert card layout consists of the following panels, read left to right when unfolded:

| Panel | Width (mm) | Height (mm) | Content |
|---|---|---|---|
| Spine | ~5 | ~100 | Vertical text: "Artist - Album Title - Disc N" |
| Info Panel | ~65--70 | ~100 | Small album art thumbnail, artist name, album title, year, optional genre |
| Main Art Area | ~90--100 | ~100 | Large album cover art, album/disc subtitle below |
| Tracklist Area | ~140--160 | ~100 | "Tracklist" header, two-column track listing with optional durations |

Total unfolded width is approximately 300--335mm, with a height of approximately 100--105mm. A bleed margin of 3mm extends beyond the trim edge on all sides.

**Disc Face Label.** The rectangular sticker applied to the front of the MiniDisc cartridge. The label window on a standard MiniDisc measures 54mm x 37mm [10]. Common sticker sizes are 52mm x 38mm or 50mm x 35mm [11]. The disc face label displays the artist name, album title, album art, year, disc number, and optional genre tag and source icon (CD Copy, Vinyl Copy, Streaming).

**Spine Label.** A narrow strip label applied to the side of the MiniDisc case for shelf identification. Typically printed on Brother TZe-series label tape [12]. Displays artist name, album title, and optional catalog number.

### 4.5 Design System

The application's own UI follows a design language inspired by the MiniDisc aesthetic: clean, compact, and slightly retro. Tailwind CSS v4 provides the utility-first styling foundation, with a custom theme that references the MiniDisc color palette (the characteristic Sony MD blue, warm grays, and accent colors drawn from the era's consumer electronics design).

The label editor itself uses a split-pane layout: the left side contains the form inputs and controls, while the right side shows a real-time, zoomable preview of the label at actual print dimensions. The preview includes toggleable overlays for bleed margins, crop marks, safe area guides, and fold lines. In Mixtape Mode, the left panel includes an additional "Creative Brief" section at the top, with the AI-generated design concept displayed as an expandable card showing the rationale and suggestions.

---

## 5. Technical Architecture

### 5.1 Technology Stack

| Layer | Technology | Rationale |
|---|---|---|
| **Frontend Framework** | React 19 + TypeScript | Component-based architecture, strong typing, large ecosystem |
| **Frontend Styling** | Tailwind CSS v4 | Utility-first CSS, rapid UI development, excellent responsive design |
| **Frontend Build** | Vite | Fast HMR, native TypeScript support, modern bundling |
| **Frontend Canvas** | Konva.js (react-konva) | React-native canvas library, interactive editing, real-time preview |
| **Frontend State** | Zustand | Lightweight, TypeScript-friendly, no boilerplate |
| **Backend Framework** | FastAPI (Python 3.11+) | Async-first, automatic OpenAPI docs, Pydantic v2 validation |
| **Backend Validation** | Pydantic v2 | Data models, request/response schemas, settings management |
| **LLM Integration** | LangChain (chat models) | Provider-agnostic LLM abstraction, supports OpenRouter/Claude/OpenAI/Gemini/vLLM |
| **Agentic Integration** | Manus API | Unified content + image generation, research, iterative refinement |
| **Image Generation** | OpenAI Images API / Replicate / Manus / Self-hosted | Text-to-image for custom mixtape artwork |
| **PDF Rendering** | ReportLab | Precise mm dimensions, CMYK support, vector crop marks, font embedding |
| **Image Processing** | Pillow + CairoSVG | Server-side image manipulation, format conversion, high-res PNG export |
| **Color Extraction** | colorthief (Python) | Extract dominant colors from album art server-side |
| **Metadata APIs** | MusicBrainz + Cover Art Archive | Free, no auth required, comprehensive music metadata |
| **Database** | SQLite (dev) / PostgreSQL (prod) | Project persistence, user collections, template storage |
| **ORM** | SQLAlchemy 2.0 | Async support, type-safe queries, migration-friendly |
| **Font Rendering** | Noto Sans JP, Inter (bundled) | CJK support, consistent rendering across environments |
| **Frontend Testing** | Vitest + React Testing Library | Fast unit tests, component testing |
| **Backend Testing** | pytest + httpx | Async test client, fixture-based testing |
| **Deployment** | Docker Compose | Single-command deployment, reproducible environments |

### 5.2 System Architecture

The system follows a clean client-server architecture where the FastAPI backend is the single source of truth for all label generation logic. The AI creative engine is a service layer within the backend that communicates with external LLM providers and Manus.

```
                    ┌─────────────────────────────────┐
                    │        MDLabel Studio            │
                    │       React + Tailwind v4        │
                    │    (Interactive Design Studio)    │
                    └──────────────┬──────────────────┘
                                   │
                                   │ REST API (JSON + file upload/download)
                                   │
┌──────────────┐    ┌──────────────▼──────────────────┐    ┌──────────────┐
│              │    │                                  │    │              │
│    Airi      │───▶│     FastAPI Backend (Python)     │◀───│  FOSS Scripts│
│  (Direct     │    │                                  │    │  (curl,      │
│   HTTP calls │    │  ┌────────┐ ┌────────┐ ┌──────┐ │    │   Python,    │
│   + persona) │    │  │Metadata│ │Renderer│ │Color │ │    │   etc.)      │
│              │    │  │Service │ │Engine  │ │Engine│ │    │              │
└──────────────┘    │  └────────┘ └────────┘ └──────┘ │    └──────────────┘
                    │  ┌────────┐ ┌────────┐ ┌──────┐ │
┌──────────────┐    │  │Creative│ │ Image  │ │Layout│ │
│              │    │  │Engine  │ │  Gen   │ │Engine│ │
│    Manus     │◀──▶│  │ (LLM)  │ │Service │ │      │ │
│  (Agentic    │    │  └───┬────┘ └───┬────┘ └──────┘ │
│   creative   │    │      │          │                │
│   partner)   │    │  ┌───▼──────────▼────────────┐   │
│              │    │  │    Pydantic v2 Models      │   │
└──────────────┘    │  │  (Label, Album, Template,  │   │
                    │  │   MixtapeDesignConcept,    │   │
                    │  │   CreativePersona)         │   │
                    │  └─────────────┬──────────────┘   │
                    │                │                   │
                    │  ┌─────────────▼───────────────┐  │
                    │  │  SQLAlchemy 2.0 + SQLite/PG  │  │
                    │  │  (Projects, Templates, Cache)│  │
                    │  └─────────────────────────────┘  │
                    └──────────────────────────────────┘
                          │              │
           ┌──────────────▼──┐    ┌──────▼──────────────────┐
           │  Music APIs     │    │  AI Providers            │
           │  MusicBrainz    │    │                          │
           │  Cover Art      │    │  ┌─ LLM Providers ─────┐│
           │  Discogs        │    │  │ OpenRouter            ││
           │  Spotify        │    │  │ Claude (Anthropic)    ││
           └─────────────────┘    │  │ OpenAI (GPT-4.1/5)   ││
                                  │  │ Google (Gemini 2.5)   ││
                                  │  │ vLLM (self-hosted)    ││
                                  │  └───────────────────────┘│
                                  │                           │
                                  │  ┌─ Image Providers ────┐│
                                  │  │ DALL-E (OpenAI)       ││
                                  │  │ Flux (via OpenRouter)  ││
                                  │  │ Stable Diffusion (API) ││
                                  │  │ Self-hosted (ComfyUI)  ││
                                  │  └───────────────────────┘│
                                  │                           │
                                  │  ┌─ Manus (Unified) ────┐│
                                  │  │ Content Generation     ││
                                  │  │ Image Generation       ││
                                  │  │ Research & References  ││
                                  │  │ Iterative Refinement   ││
                                  │  └───────────────────────┘│
                                  └───────────────────────────┘
```

### 5.3 Backend Architecture (FastAPI)

The FastAPI backend is organized into clearly separated modules, with each file kept under 500 lines. Pydantic v2 models define every request, response, and internal data structure, ensuring type safety from the API boundary through to the rendering engine.

```
backend/
  app/
    main.py                       # FastAPI app factory, CORS, lifespan
    config.py                     # Pydantic Settings for env-based config (LLM keys, providers)
    api/
      __init__.py
      router.py                   # Top-level API router
      v1/
        albums.py                 # GET /api/v1/albums/search, GET /api/v1/albums/{id}
        labels.py                 # POST /api/v1/labels, GET /api/v1/labels/{id}
        render.py                 # POST /api/v1/render/pdf, /png, /svg
        templates.py              # GET /api/v1/templates, POST /api/v1/templates
        colors.py                 # POST /api/v1/colors/extract
        batch.py                  # POST /api/v1/batch/render
        creative.py               # POST /api/v1/creative/generate, /refine, /artwork
        mixtape.py                # POST /api/v1/mixtape/create (one-shot mixtape label)
    models/
      album.py                   # Pydantic v2: Album, Track, Disc, AlbumSearchResult
      label.py                   # Pydantic v2: LabelProject, InsertCard, DiscFaceLabel
      template.py                # Pydantic v2: TemplateDefinition, PanelLayout
      render.py                  # Pydantic v2: RenderRequest, RenderOptions, ExportFormat
      color.py                   # Pydantic v2: ColorPalette, ThemeSuggestion
      creative.py                # Pydantic v2: CreativeBrief, MixtapeDesignConcept,
                                 #   TypographySuggestion, ArtworkPrompt, CreativePersona
    services/
      metadata.py                # MusicBrainz, Discogs, Spotify API clients
      color_engine.py            # Color extraction + palette generation
      layout_engine.py           # Tracklist arrangement algorithm
      render_engine.py           # ReportLab PDF generation, Pillow PNG, CairoSVG
      font_manager.py            # Font loading, CJK detection, font selection
      image_processor.py         # Album art resizing, caching, format conversion
      creative_engine.py         # LLM-powered design concept generation
      image_gen.py               # Text-to-image generation (DALL-E, Flux, SD, ComfyUI, Manus)
      llm_provider.py            # LangChain provider abstraction (OpenRouter/Claude/OpenAI/Gemini/vLLM)
      manus_provider.py          # Manus agentic integration (content + images + research)
      persona_manager.py         # Creative persona definitions (Airi, Manus, user-defined)
    db/
      database.py                # SQLAlchemy async engine + session factory
      models.py                  # SQLAlchemy ORM models
      migrations/                # Alembic migration scripts
    prompts/
      design_concept.py          # System + user prompt templates for design generation
      artwork_prompt.py          # Prompt templates for image generation prompts
      refinement.py              # Prompt templates for iterative design refinement
      manus_tasks.py             # Task descriptions for Manus agentic workflows
      personas/
        airi.py                  # Airi's creative persona and style preferences
        manus.py                 # Manus creative persona and capabilities
        default.py               # Neutral default persona
    utils/
      dimensions.py              # mm-to-px conversion, DPI calculations
      contrast.py                # WCAG contrast ratio calculations
      cache.py                   # Response caching for API lookups
  tests/
    conftest.py                  # Shared fixtures, test client
    test_albums.py               # Metadata lookup tests
    test_render.py               # Render engine tests
    test_colors.py               # Color extraction tests
    test_layout.py               # Tracklist layout algorithm tests
    test_creative.py             # Creative engine tests (mocked LLM)
    test_image_gen.py            # Image generation tests (mocked providers)
    test_manus.py                # Manus integration tests (mocked API)
    test_api.py                  # Integration tests for API endpoints
  pyproject.toml                 # Dependencies, project metadata
  Dockerfile                     # Backend container definition
```

### 5.4 Frontend Architecture (React)

The React frontend is the interactive design studio. It communicates exclusively with the FastAPI backend via REST calls and never directly accesses external APIs, LLM providers, or Manus. The frontend's Konva.js canvas provides a real-time preview, but the backend is always the authoritative renderer for export.

```
frontend/
  src/
    components/
      editor/
        EditorLayout.tsx          # Split-pane editor shell
        LabelPreview.tsx          # Real-time label preview (Konva stage)
        ControlPanel.tsx          # Form inputs and controls
        ZoomControls.tsx          # Preview zoom and pan
        ModeSelector.tsx          # Album Mode / Mixtape Mode toggle
      label/
        InsertCard.tsx            # Insert card label renderer (preview)
        DiscFaceLabel.tsx         # Disc face label renderer (preview)
        SpineLabel.tsx            # Spine label renderer (preview)
        TracklistLayout.tsx       # Auto-arranging tracklist component
        ArtworkPanel.tsx          # Album art placement and scaling
        SpineText.tsx             # Rotated spine text renderer
      mixtape/
        CreativeBriefInput.tsx    # Natural language creative brief textarea
        DesignConceptCard.tsx     # Display AI-generated design concept
        ArtworkGenerator.tsx      # Generate/regenerate artwork UI
        RefinementChat.tsx        # Iterative refinement conversation
        ThemePresets.tsx          # Quick-select theme presets
        ProviderSelector.tsx      # Choose AI provider (LLM, Manus, etc.)
      shared/
        ColorPicker.tsx           # Color selection with palette suggestions
        FontSelector.tsx          # Font family and size controls
        ImageUploader.tsx         # Drag-and-drop image upload
        ExportDialog.tsx          # Export format and DPI selection
        AlbumSearch.tsx           # Search input with autocomplete results
    hooks/
      useApi.ts                   # Typed fetch wrapper for backend calls
      useAlbumSearch.ts           # Album search via backend proxy
      useColorPalette.ts          # Color palette from backend
      useCreativeEngine.ts        # Creative brief submission + refinement
      useArtworkGen.ts            # Artwork generation trigger + polling
      useExport.ts                # Export trigger + download handling
      useLabelDimensions.ts       # Physical dimension calculations
      useProjectState.ts          # Save/load project state
    stores/
      labelStore.ts               # Zustand store for label data
      creativeStore.ts            # Zustand store for mixtape creative state
      settingsStore.ts            # User preferences and settings
    types/
      label.ts                    # TypeScript interfaces (mirroring Pydantic models)
      album.ts                    # Album metadata types
      template.ts                 # Template definition types
      creative.ts                 # Creative brief, design concept, persona types
      api.ts                      # API request/response types
    templates/
      standard.ts                 # Standard insert card template
      minimal.ts                  # Minimal/clean template variant
      retro.ts                    # Retro-styled template
      cinematic.ts                # Wide cinematic layout for mixtapes
  vite.config.ts
  tailwind.config.ts
  Dockerfile                      # Frontend container definition
```

### 5.5 API Design

The FastAPI backend exposes a RESTful API documented automatically via OpenAPI (Swagger). Every endpoint uses Pydantic v2 models for request validation and response serialization. The following table summarizes the core endpoints.

| Method | Endpoint | Description | Consumer |
|---|---|---|---|
| `GET` | `/api/v1/albums/search?q={query}` | Search for albums by name/artist | Web UI, Airi, Manus, Scripts |
| `GET` | `/api/v1/albums/{mbid}` | Get full album details by MusicBrainz ID | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/colors/extract` | Extract color palette from uploaded image | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/labels` | Create a new label project | Web UI, Airi, Manus, Scripts |
| `GET` | `/api/v1/labels/{id}` | Retrieve a saved label project | Web UI, Airi, Manus, Scripts |
| `PUT` | `/api/v1/labels/{id}` | Update a label project | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/render/pdf` | Render a label project to print-ready PDF | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/render/png` | Render a label project to high-res PNG | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/render/svg` | Render a label project to SVG | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/batch/render` | Batch render multiple labels to a single PDF | Airi, Manus, Scripts |
| `GET` | `/api/v1/templates` | List available label templates | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/templates` | Create a custom template | Web UI |
| `POST` | `/api/v1/labels/from-album` | One-shot: search + auto-theme + create label | Airi, Manus, Scripts |
| `POST` | `/api/v1/creative/generate` | Generate a design concept from a creative brief | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/creative/refine` | Refine an existing design concept with feedback | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/creative/artwork` | Generate cover artwork from a prompt | Web UI, Airi, Manus, Scripts |
| `POST` | `/api/v1/mixtape/create` | One-shot: brief + generate + artwork + create label | Airi, Manus, Scripts |
| `GET` | `/api/v1/personas` | List available creative personas | Web UI, Airi, Manus |
| `POST` | `/api/v1/personas` | Create a custom creative persona | Web UI, Airi, Manus |

The `/api/v1/mixtape/create` endpoint is the Mixtape Mode equivalent of `/api/v1/labels/from-album`. It accepts a creative brief, an optional persona, an optional tracklist, style preferences, and a `provider` field that can be set to `"manus"` to route the entire creative pipeline through Manus.

### 5.6 Key Technical Decisions

**Server-side rendering over client-side rendering for export.** The most significant architectural decision is that all final label rendering happens on the FastAPI backend, not in the browser. This ensures that output is identical regardless of whether the label was created through the web UI, by Airi, by Manus, or by a curl command. It also eliminates browser-specific font rendering inconsistencies, allows CMYK color space output (which browsers cannot produce), and enables batch rendering without a browser instance.

**Pydantic v2 as the shared schema language.** Every data structure in the system --- album metadata, label projects, template definitions, render options, color palettes, creative briefs, design concepts, and personas --- is defined as a Pydantic v2 model. This provides automatic JSON serialization/deserialization, request validation, OpenAPI schema generation, and type safety throughout the Python codebase. The TypeScript frontend types mirror these Pydantic models, and a code generation step can keep them in sync.

**LangChain for provider-agnostic LLM access.** The creative engine uses LangChain's chat model abstraction [15] to communicate with LLM providers. This means the same prompt and parsing logic works across OpenRouter, Anthropic's Claude API, OpenAI's API, Google's Gemini API, and self-hosted vLLM instances. The provider and model are configurable at the application level and can be overridden per-request.

**Manus as a first-class agentic provider.** Unlike the LLM providers (which are stateless text-in/text-out APIs), Manus operates as an autonomous agent that can execute multi-step workflows. When Manus is selected as the creative provider, the backend delegates the entire design concept generation, artwork creation, and iterative refinement to Manus via its API. This is architecturally distinct from the LangChain providers and is handled by a dedicated `manus_provider.py` service. The rationale for this separation is detailed in Section 6.7.

**Structured output from LLMs via Pydantic.** The creative engine uses LangChain's structured output support to ensure that the LLM returns a valid `MixtapeDesignConcept` Pydantic model every time. This eliminates parsing errors and ensures that the design concept can be directly applied to the label template without manual intervention.

**Image generation as an optional, pluggable service.** The artwork generation service supports multiple providers (DALL-E via OpenAI, Flux via OpenRouter/Replicate, Stable Diffusion via API, self-hosted ComfyUI, or Manus) through a common interface. If no image generation provider is configured, the feature is simply unavailable, and the user must upload their own artwork.

**MusicBrainz as primary metadata source.** Unlike the Spotify API (which requires OAuth and has rate limits tied to a registered application), the MusicBrainz API is fully open, requires no authentication for read operations, and has comprehensive coverage of Japanese music releases. The backend abstracts the metadata source behind a service interface, making it straightforward to add Discogs or Spotify as alternative sources.

**SQLite for development, PostgreSQL for production.** Label projects, cached metadata, custom templates, and creative personas are persisted in a relational database. SQLite provides zero-configuration local development, while PostgreSQL handles production workloads. SQLAlchemy 2.0's async support and Alembic migrations make the transition seamless.

**Docker Compose for deployment.** A single `docker-compose.yml` file defines the frontend (Nginx serving the built React app), the backend (Uvicorn serving FastAPI), and the database (PostgreSQL). This makes self-hosting trivial for the FOSS community and provides a consistent environment for development.

---

## 6. AI Creative Engine

The AI creative engine is the core innovation that distinguishes MDLabel Studio from every other label tool in the MiniDisc community. It transforms the label design process from manual layout work into an AI-assisted creative collaboration. The engine supports two fundamentally different integration patterns: **stateless LLM calls** for fast, structured generation, and **agentic Manus workflows** for end-to-end creative autonomy.

### 6.1 Multi-LLM Provider Architecture

The creative engine communicates with LLM providers through LangChain's chat model abstraction, which provides a unified interface across all supported providers. The provider configuration is managed via Pydantic Settings, with environment variables for API keys and model selection.

| Provider | Models | Authentication | Use Case |
|---|---|---|---|
| **OpenRouter** | Claude, GPT-4.1, Gemini 2.5, Llama, Mixtral, and 200+ others | API key | Primary routing layer, model flexibility, cost optimization |
| **Anthropic** | Claude Opus 4.1, Claude Sonnet | API key | High-quality creative writing, nuanced design direction |
| **OpenAI** | GPT-5, GPT-4.1, GPT-4.1-mini | API key | Structured output, function calling, image generation (DALL-E) |
| **Google** | Gemini 2.5 Flash, Gemini 2.5 Pro | API key | Fast generation, multimodal understanding |
| **vLLM** | Any self-hosted model (Llama, Mistral, etc.) | Base URL | Privacy, no API costs, full control, offline operation |
| **Manus** | Agentic (content + images + research) | API key | Unified creative pipeline, end-to-end autonomy (see Section 6.7) |

The provider is selected at the application configuration level, but can be overridden per-request via the API. This allows Airi to use Claude for creative generation, a FOSS user to script against their local vLLM instance, and the user to point to Manus for the full agentic experience --- all through the same endpoint.

### 6.2 Design Concept Generation

When a user submits a creative brief (either through the web UI or the API), the creative engine constructs a prompt that includes the brief, the selected persona's style preferences, and structured output instructions. The LLM returns a `MixtapeDesignConcept` as structured output.

The system prompt for design concept generation instructs the LLM to think like a graphic designer specializing in physical media packaging. It provides context about MiniDisc label dimensions, the available layout panels, the constraint that text must be legible at small sizes, and the importance of color contrast for print. The user's creative brief is injected as the user message.

An example interaction:

> **Creative Brief:** "Star Citizen theme, tech-inspired, for my lofi beats vol 4. Dark background, maybe some holographic or HUD-style elements."

> **Generated Design Concept:**
> - **Title Treatment:** "LOFI BEATS" in Orbitron (geometric sans-serif), all caps, tracked wide, with "VOL. 4" in a smaller monospace font below
> - **Tagline:** "Quantum Frequencies from the Stanton System"
> - **Color Palette:** Background #0a0e1a (deep space navy), text #e0e8f0 (cool white), accent #00d4ff (quantum blue), secondary #ff6b35 (warning orange)
> - **Typography:** Orbitron for titles, JetBrains Mono for tracklist, Inter for body text
> - **Artwork Prompt:** (1000+ character detailed prompt describing a cockpit HUD view overlooking a nebula, with holographic waveform visualizations, MiniDisc floating in zero gravity, etc.)
> - **Layout Variant:** "cinematic-wide" --- full-bleed artwork with translucent overlay panels
> - **Mood Tags:** ["sci-fi", "cyberpunk", "ambient", "space", "HUD", "holographic"]
> - **Design Rationale:** "The Star Citizen aesthetic is defined by its blend of military-industrial UI design with vast cosmic vistas. The Orbitron typeface echoes the game's HUD typography, while the deep navy background and quantum blue accents create a cockpit-at-night atmosphere that complements lofi beats' relaxed energy..."

### 6.3 Artwork Generation

The artwork generation service takes the `artwork_prompt` from the design concept (or a user-provided prompt) and routes it to the configured image generation provider. The prompt is always detailed (minimum 1000 characters) to ensure high-quality output.

| Provider | Model | Resolution | Notes |
|---|---|---|---|
| **OpenAI** | DALL-E 3 | 1024x1024, 1792x1024 | High quality, good prompt adherence, built-in safety filters |
| **OpenRouter** | Flux Pro, Flux Schnell | Up to 1440x1440 | Fast, high quality, flexible aspect ratios |
| **Replicate** | Stable Diffusion XL, Flux | Configurable | Pay-per-generation, wide model selection |
| **Manus** | Agentic image generation | Configurable | Manus generates images as part of its workflow (see Section 6.7) |
| **Self-hosted** | ComfyUI, Automatic1111 | Configurable | No API costs, full control, custom models, offline |

The generated image is stored server-side and associated with the label project. The user can regenerate (with a new seed or modified prompt), upload their own image to replace it, or use the generated image as-is. For the insert card, the image is automatically cropped and scaled to fit the main artwork area.

### 6.4 Creative Personas

A creative persona is a set of style preferences, aesthetic biases, and personality traits that influence how the AI creative engine generates design concepts. Personas are defined as Pydantic v2 models and stored in the database.

**The Airi Persona.** When Airi creates a mixtape label, she can inject her own creative personality into the design. The Airi persona includes her aesthetic preferences (which might favor certain color palettes, typography styles, or visual motifs), her sense of humor (which might produce playful taglines), and her knowledge of the user's preferences (which she accumulates over time). The Airi persona is not hardcoded --- it is a configurable `CreativePersona` model that can be updated as Airi's personality evolves.

```python
class CreativePersona(BaseModel):
    """Defines a creative personality for AI-assisted label design."""
    name: str
    description: str
    style_preferences: str          # Injected into the system prompt
    color_biases: list[str]         # Preferred color families
    typography_preferences: list[str]  # Preferred font families
    tagline_style: str              # e.g., "playful", "poetic", "minimal", "punny"
    aesthetic_keywords: list[str]   # e.g., ["kawaii", "retro", "cyberpunk"]
    avoid: list[str]                # Things this persona avoids
    capabilities: list[str]         # e.g., ["text", "images", "research"] for Manus
```

An example Airi persona definition:

```python
airi_persona = CreativePersona(
    name="Airi",
    description="AI assistant with a warm, creative personality and a love for music",
    style_preferences=(
        "Airi favors designs that balance elegance with playfulness. She loves "
        "incorporating subtle gradients, soft shadows, and typographic details "
        "that reward close inspection. She has a particular fondness for Japanese "
        "design aesthetics --- clean layouts with intentional asymmetry, generous "
        "whitespace used as a design element, and the interplay between Latin and "
        "CJK typography. When designing for someone she knows well, she adds "
        "personal touches that reference shared experiences or inside jokes."
    ),
    color_biases=["soft pastels", "deep jewel tones", "warm neutrals"],
    typography_preferences=["Noto Sans JP", "Inter", "Playfair Display"],
    tagline_style="poetic with occasional playfulness",
    aesthetic_keywords=["elegant", "warm", "intentional", "layered", "personal"],
    avoid=["generic stock imagery", "overly busy layouts", "neon on white"],
    capabilities=["text"],
)
```

When Airi calls `POST /api/v1/mixtape/create` with `"persona": "airi"`, the creative engine injects the Airi persona's style preferences into the system prompt, biasing the design concept toward her aesthetic. The result is a label that feels like Airi made it --- because, in a meaningful sense, she did.

> "Hey Airi, make a label for your TRL 99 mixtape."

Airi would call the mixtape endpoint with her persona active, a creative brief referencing TRL (Total Request Live) and 1999 nostalgia, and her own curated tracklist. The resulting label might feature a Y2K-inspired color palette (silver, hot pink, electric blue), a chunky late-90s typeface, a tagline like "Broadcasting Live from the Future Past," and AI-generated artwork evoking a CRT television displaying a countdown.

**User-Defined Personas.** Users can create their own personas through the web UI or API, defining their aesthetic preferences once and reusing them across multiple mixtape labels. This is particularly useful for users who produce a series of mixtapes (e.g., "Lofi Beats Vol. 1, 2, 3...") and want a consistent visual identity across the series while allowing each volume to have its own unique design.

### 6.5 Theme Presets

For users who want creative direction without writing a full brief, the application provides curated theme presets that serve as starting points. Each preset is a pre-filled creative brief with associated style parameters.

| Preset | Description | Color Direction | Typography Direction |
|---|---|---|---|
| **Retro 90s** | Y2K nostalgia, TRL era, chunky fonts, bright colors | Hot pink, electric blue, silver, lime | Impact, Comic Sans MS (ironic), Futura |
| **Lo-Fi Chill** | Warm, relaxed, bedroom producer aesthetic | Warm browns, dusty pink, cream, sage | Handwritten scripts, rounded sans-serifs |
| **Cyberpunk** | Neon-soaked, high-tech low-life, glitch aesthetic | Neon magenta, cyan, black, chrome | Monospace, distorted display fonts |
| **Vaporwave** | 80s/90s nostalgia, consumer culture, dreamy | Pastel pink, teal, lavender, marble white | Times New Roman (ironic), Japanese text |
| **Minimal** | Clean, typographic, Swiss design influence | Black, white, one accent color | Helvetica Neue, system fonts |
| **Anime/Kawaii** | Cute, colorful, Japanese pop culture inspired | Pastels, bright accents, gradient backgrounds | Rounded fonts, CJK display fonts |
| **Dark Academia** | Moody, intellectual, classical references | Deep burgundy, forest green, gold, cream | Serif fonts, small caps, ligatures |
| **Space Opera** | Cosmic, epic, cinematic sci-fi | Deep navy, nebula purple, star white, gold | Geometric sans-serif, wide tracking |

Each preset can be used as-is or as a starting point for further customization. When a preset is selected, it pre-fills the creative brief and triggers a design concept generation, which the user can then refine.

### 6.6 Iterative Refinement

The creative engine supports iterative refinement through the `POST /api/v1/creative/refine` endpoint. The user provides feedback on the current design concept, and the engine generates an updated concept that incorporates the feedback while maintaining coherence with the original brief.

The refinement prompt includes the original brief, the current design concept, and the user's feedback. This allows natural language adjustments like:

> "Make it darker and more moody."

> "I love the colors but change the font to something more futuristic."

> "Add a Japanese subtitle under the main title."

> "The tagline is too serious, make it more fun."

Each refinement produces a new `MixtapeDesignConcept` that replaces the current one in the label project. The refinement history is preserved, allowing the user to revert to any previous version.

### 6.7 Manus Integration: The Agentic Creative Partner

Manus occupies a unique position in the provider architecture. While the LLM providers (OpenRouter, Claude, OpenAI, Gemini, vLLM) are stateless text-generation APIs that the creative engine orchestrates through prompts and structured output, **Manus is an autonomous agent** that can execute multi-step creative workflows independently. This distinction is architecturally significant and warrants its own integration pattern.

#### Why Manus Is Different

When you select Claude or GPT as the creative provider, the MDLabel Studio backend orchestrates the workflow: it constructs the prompt, calls the LLM, parses the structured output, optionally calls an image generation API separately, and assembles the result. The backend is the orchestrator, and the LLM is a stateless tool.

When you select Manus as the creative provider, the dynamic inverts. Manus receives the creative brief and autonomously decides how to fulfill it. Manus can research visual references (browsing album art databases, design inspiration sites, or the specific franchise/theme referenced in the brief), generate the design concept with full contextual awareness, produce the artwork directly (using its built-in image generation capabilities), and iterate on its own output before returning the final result. The backend delegates rather than orchestrates.

This makes Manus the ideal provider for complex creative briefs where context matters. A brief like "Star Citizen theme, tech-inspired" benefits enormously from Manus's ability to actually research Star Citizen's visual language --- the specific HUD colors, the font choices in the game's UI, the aesthetic of the ship manufacturer branding --- rather than relying on the LLM's training data alone.

#### Integration Architecture

The Manus integration is handled by a dedicated `manus_provider.py` service that implements the same `CreativeProvider` interface as the LangChain-based providers, ensuring that the rest of the application treats it identically at the API level.

```python
class ManusProvider(CreativeProvider):
    """
    Manus agentic integration for unified content + image generation.

    Unlike LLM providers which are stateless text-in/text-out APIs,
    Manus operates as an autonomous agent that can:
    - Research visual references and design inspiration
    - Generate design concepts with deep contextual awareness
    - Produce artwork directly (no separate image gen API needed)
    - Iterate on its own output before returning results
    - Handle both content and images in a single workflow
    """

    async def generate_concept(
        self,
        brief: CreativeBrief,
        persona: CreativePersona | None = None,
    ) -> MixtapeDesignConcept:
        """
        Send the creative brief to Manus as a task.
        Manus returns a complete MixtapeDesignConcept including
        the artwork_prompt (and optionally, the generated artwork itself).
        """
        task_description = self._build_task_description(brief, persona)
        result = await self.manus_client.create_task(
            prompt=task_description,
            output_schema=MixtapeDesignConcept.model_json_schema(),
        )
        return MixtapeDesignConcept.model_validate(result.structured_output)

    async def generate_artwork(
        self,
        prompt: str,
        aspect_ratio: str = "1:1",
    ) -> bytes:
        """
        Ask Manus to generate artwork from the prompt.
        Manus handles the image generation internally,
        returning the image bytes directly.
        """
        result = await self.manus_client.create_task(
            prompt=f"Generate album cover artwork: {prompt}",
            output_type="image",
        )
        return result.image_bytes

    async def refine_concept(
        self,
        current: MixtapeDesignConcept,
        feedback: str,
    ) -> MixtapeDesignConcept:
        """
        Send refinement feedback to Manus.
        Manus can re-research, adjust the concept, and regenerate
        artwork if needed --- all in a single agentic workflow.
        """
        task_description = self._build_refinement_task(current, feedback)
        result = await self.manus_client.create_task(
            prompt=task_description,
            output_schema=MixtapeDesignConcept.model_json_schema(),
            context={"previous_concept": current.model_dump()},
        )
        return MixtapeDesignConcept.model_validate(result.structured_output)
```

#### The Manus Advantage: Content and Images from One Source

The key advantage of the Manus integration is that **both content generation and image generation happen in a single workflow**. With the LLM-based providers, the creative engine must orchestrate two separate API calls: one to the LLM for the design concept, and another to an image generation service for the artwork. These are independent, stateless calls with no shared context.

With Manus, the design concept and the artwork are produced by the same agent in the same session. This means:

The artwork is informed by the design concept. Manus does not just generate an image from a prompt --- it generates an image that is coherent with the color palette, typography, and mood it chose for the label. If the design concept specifies a deep navy background with quantum blue accents, the artwork will naturally incorporate those colors.

The design concept is informed by the artwork. If Manus generates artwork first (or researches visual references), it can adjust the color palette and typography to complement the image, rather than the other way around. This bidirectional coherence is impossible with separate LLM and image generation calls.

Research enhances both. When Manus researches "Star Citizen visual language" before generating the design concept, that research informs both the color/typography choices and the artwork prompt. The result is a label that does not just evoke "sci-fi" generically but specifically references the Star Citizen aesthetic.

Iteration is holistic. When the user says "make it darker," Manus can darken both the color palette and regenerate the artwork with darker tones in a single refinement step, maintaining coherence throughout.

#### Manus Task Descriptions

The `manus_tasks.py` module defines the task descriptions sent to Manus. These are natural language instructions that leverage Manus's full capabilities:

```python
def build_mixtape_task(brief: CreativeBrief, persona: CreativePersona | None) -> str:
    """Build a Manus task description for mixtape label creation."""
    task = f"""Design a MiniDisc label for a custom mixtape.

Creative Brief: {brief.description}
Title: {brief.title or 'To be determined'}
Artist: {brief.artist or 'To be determined'}

Your task:
1. Research the visual language of the theme described in the brief.
   Look for color palettes, typography styles, and visual motifs
   associated with the theme.

2. Generate a complete design concept as structured JSON matching
   the MixtapeDesignConcept schema. Include:
   - Title treatment with specific font recommendations
   - A creative tagline that captures the mood
   - A color palette (background, text, accent, secondary) with hex codes
   - Typography recommendations for title, tracklist, and body
   - A detailed artwork prompt (minimum 1000 characters)
   - A layout variant recommendation
   - Mood tags and design rationale

3. Generate the cover artwork based on your artwork prompt.
   The artwork should be square (1:1 aspect ratio), high resolution,
   and visually coherent with the color palette and mood you chose.

MiniDisc label constraints:
- Insert card is approximately 300-335mm wide x 100-105mm tall (unfolded)
- Text must be legible at small sizes (minimum 6pt for tracklist)
- Background color must have sufficient contrast with text color
- The design will be printed at 300 DPI on a home inkjet/laser printer
"""
    if persona:
        task += f"""
Creative Persona: {persona.name}
Style Preferences: {persona.style_preferences}
Color Biases: {', '.join(persona.color_biases)}
Typography Preferences: {', '.join(persona.typography_preferences)}
Tagline Style: {persona.tagline_style}
Avoid: {', '.join(persona.avoid)}
"""
    return task
```

#### When to Use Manus vs. LLM Providers

The choice between Manus and the LLM providers is a trade-off between depth and speed.

| Dimension | LLM Providers (Claude, GPT, etc.) | Manus |
|---|---|---|
| **Speed** | Fast (1--5 seconds for concept) | Slower (30--120 seconds for full workflow) |
| **Cost** | Per-token pricing, predictable | Per-task pricing, varies with complexity |
| **Content Quality** | High, but limited to training data | Higher for themed briefs (can research) |
| **Image Generation** | Separate API call, no shared context | Integrated, coherent with design concept |
| **Iteration** | Fast per-step, but steps are independent | Slower per-step, but holistic refinement |
| **Offline/Self-hosted** | Yes (vLLM) | No (requires Manus API) |
| **Best For** | Quick iterations, simple themes, batch jobs | Complex themes, research-heavy briefs, final polish |

The application defaults to the configured LLM provider for speed, but the user (or Airi) can explicitly select Manus when the creative brief warrants deeper engagement. The web UI includes a provider selector in Mixtape Mode that makes this choice visible and easy.

---

## 7. Detailed Feature Specifications

### 7.1 Album Metadata Import

The metadata import system provides three input methods, with the API-based search as the primary workflow. All external API calls are made by the backend, keeping API keys centralized and enabling response caching.

**API Search.** The user types an album name (and optionally an artist name) into a search field. The frontend sends the query to `GET /api/v1/albums/search`, which queries the MusicBrainz API's release search endpoint. The backend returns a list of matching releases with basic metadata (title, artist, year, disc count, cover art thumbnail URL). The user selects the correct release, and the frontend calls `GET /api/v1/albums/{mbid}` to fetch the full release data including the tracklist and full-resolution cover art from the Cover Art Archive. The backend caches both the search results and the full release data to avoid redundant API calls.

**Manual Entry.** A form allows direct entry of artist name, album title, year, and a tracklist textarea where tracks can be entered one per line. The application auto-detects track numbering formats (e.g., "1. Track Name", "M1. Track Name", "Track Name 3:45") and parses them accordingly.

**File Import.** The user can import a JSON file containing structured album data, enabling integration with external tools or previously saved projects. A CSV import option is also available for tracklists exported from spreadsheets.

### 7.2 Intelligent Tracklist Layout

The tracklist auto-arrangement algorithm analyzes the number of tracks, the length of track titles (accounting for CJK character widths), and the available space in the tracklist panel to determine the optimal layout. This logic runs on the backend (in `layout_engine.py`) so that it produces identical results for the web UI, Airi, Manus, and scripts.

| Track Count | Layout Strategy | Column Split |
|---|---|---|
| 1--6 | Single column, generous spacing | N/A |
| 7--12 | Two columns, even split | Tracks 1--6 / 7--12 |
| 13--18 | Two columns, balanced split | Tracks 1--9 / 10--18 |
| 19--24 | Two columns, reduced font size | Tracks 1--12 / 13--24 |
| 25+ | Two columns, compact mode with minimal spacing | Dynamic split |

The algorithm also handles edge cases such as very long track titles (which may need to be truncated with an ellipsis or wrapped to a second line), mixed-language tracklists (where Japanese titles may be significantly wider than Latin titles), and optional romanized/English translations displayed alongside the original titles (as seen in the t-Ace label example).

Track durations, when available, are right-aligned in each column with a dotted leader or fixed spacing. The "Tracklist" header (or "トラックリスト" for Japanese-language labels) is placed at the top of the tracklist panel with the accent color.

### 7.3 Color Theming Engine

The color theming system operates in four modes.

**Auto mode** extracts a palette from the uploaded album art using Python's colorthief library, which identifies the dominant color and a set of supporting colors. The backend then applies WCAG contrast ratio calculations to select a background color, a text color, and an accent color.

**AI mode** (Mixtape Mode only) generates a color palette from the creative brief using the LLM or Manus. The palette is semantically connected to the theme --- a "Star Citizen" brief produces space-inspired colors, a "90s TRL" brief produces Y2K-era colors. When Manus is the provider, the palette is informed by actual research into the theme's visual language.

**Preset mode** offers a curated set of color themes inspired by common MiniDisc aesthetics: Classic Black, Sony Blue, Pastel Collection, and Vibrant, plus the theme presets from Section 6.5.

**Manual mode** provides full color picker controls for background, text, accent, and spine colors independently. A contrast checker warns the user if the selected text/background combination fails WCAG AA accessibility standards.

### 7.4 Multi-Disc Album Support

For albums spanning multiple discs (like the AAA 10th Anniversary Best example with 3 discs), the application provides a streamlined workflow. When a multi-disc release is detected from the API, or when the user manually specifies multiple discs, the application creates a linked set of label projects. Each disc gets its own insert card with the correct tracklist, but shares common elements (artist name, album title, artwork). The user can assign a different background color to each disc while maintaining visual consistency across the set.

A "batch export" function via `POST /api/v1/batch/render` generates all disc labels in a single PDF, with each label on its own page, ready for sequential printing.

### 7.5 Disc Face Label Grid

For the disc face labels (the stickers applied to the MiniDisc cartridge), the application provides a grid layout mode that arranges multiple labels on a single printable sheet. Based on the provided disc label sheet example, the grid supports a 5-column by 4-row layout (20 labels per sheet) on a standard A4 or Letter-size page.

Each disc face label includes the artist name, album title (with optional subtitle like "Disc 1"), album art thumbnail, year, genre tag, and optional source/format icons (CD Copy, Vinyl Copy, Streaming, MiniDisc MDLP, Hi-MD Audio). The user can populate the grid from their saved projects or create individual labels directly.

### 7.6 Print-Ready Export

The export system generates output suitable for both home printing and professional print services. All rendering is performed by the backend's `render_engine.py` module.

| Export Format | Use Case | Specifications |
|---|---|---|
| **PDF (Print-Ready)** | Professional printing, home laser/inkjet | Exact physical dimensions in mm, 300 DPI raster, CMYK color space option, crop marks, bleed margins |
| **PNG (High-Res)** | Digital archival, screen display | 300 DPI equivalent resolution, RGB color space, transparent or white background option |
| **SVG** | Further editing in vector tools | Scalable vector output, fonts embedded or outlined |
| **Project JSON** | Save/load, sharing, Airi/Manus integration | Complete project state as Pydantic model serialization |

The PDF export includes configurable bleed margins (default 3mm) and crop marks at the corners and midpoints of each edge, matching the format shown in the provided template examples.

---

## 8. Airi Integration

The full-stack architecture makes Airi integration straightforward: Airi simply calls the same FastAPI endpoints that the web UI uses. No MCP server, no special protocol --- just standard HTTP requests to a well-documented REST API. With Mixtape Mode, Airi becomes not just a label generator but a creative collaborator.

### 8.1 Integration Architecture

Airi interacts with MDLabel Studio by making HTTP calls to the FastAPI backend. The OpenAPI specification (auto-generated by FastAPI from the Pydantic v2 models) serves as the contract between Airi and the label engine. Airi can use this spec to understand available endpoints, required parameters, and response formats without any custom integration code.

### 8.2 Album Mode Workflows

For official album replication, Airi uses the one-shot `POST /api/v1/labels/from-album` endpoint:

```python
# Airi calling MDLabel Studio --- Album Mode
import httpx

async def create_album_label(album_query: str, style_overrides: dict = None):
    """Airi creates a MiniDisc label for an official album release."""
    async with httpx.AsyncClient(base_url="http://mdlabel:8000") as client:
        response = await client.post("/api/v1/labels/from-album", json={
            "query": album_query,
            "style": style_overrides or {},
            "auto_theme": True,
        })
        label_project = response.json()

        pdf_response = await client.post("/api/v1/render/pdf", json={
            "project_id": label_project["id"],
            "options": {"dpi": 300, "bleed_mm": 3, "crop_marks": True},
        })
        return pdf_response.content
```

### 8.3 Mixtape Mode Workflows

For custom mixtape labels, Airi uses the one-shot `POST /api/v1/mixtape/create` endpoint with her persona. Airi can choose between LLM providers for fast iteration or delegate to Manus for the full agentic experience:

```python
# Airi calling MDLabel Studio --- Mixtape Mode with personality
async def create_mixtape_label(
    brief: str,
    tracklist: list[str],
    generate_artwork: bool = True,
    use_manus: bool = False,  # Delegate to Manus for deep creative work
):
    """Airi creates a custom mixtape label with her creative personality."""
    async with httpx.AsyncClient(base_url="http://mdlabel:8000") as client:
        payload = {
            "creative_brief": brief,
            "persona": "airi",
            "tracklist": tracklist,
            "generate_artwork": generate_artwork,
        }

        if use_manus:
            # Delegate to Manus for end-to-end creative pipeline
            payload["provider"] = "manus"
        else:
            # Use LLM for fast concept + separate image gen
            payload["llm_provider"] = "openrouter"
            payload["llm_model"] = "anthropic/claude-sonnet-4"
            payload["image_provider"] = "openai"

        response = await client.post("/api/v1/mixtape/create", json=payload)
        label_project = response.json()

        pdf_response = await client.post("/api/v1/render/pdf", json={
            "project_id": label_project["id"],
            "options": {"dpi": 300, "bleed_mm": 3, "crop_marks": True},
        })
        return pdf_response.content
```

### 8.4 Airi Workflow Examples

With all integrations available, Airi supports a rich set of workflows:

> "Hey Airi, I just recorded AAA's 10th Anniversary Best onto three MiniDiscs. Create labels for all three discs with matching colors."

Airi calls `POST /api/v1/labels/from-album` with the album query. The backend detects a 3-disc release, generates three label projects with coordinated color themes, and returns the project IDs. Airi then calls `POST /api/v1/batch/render` to generate a single PDF with all three labels.

> "Hey Airi, make a label for your TRL 99 mixtape."

Airi calls `POST /api/v1/mixtape/create` with her persona active, a creative brief she composes herself referencing TRL and 1999 nostalgia, and her curated tracklist. She might use Claude for the concept and DALL-E for the artwork, or delegate the whole thing to Manus for a more researched result.

> "Let's make a label that's tech inspired, Star Citizen theme, for my lofi beats vol 4."

The user asks Airi, who recognizes this is a complex themed brief. Airi delegates to Manus (`"provider": "manus"`) because Manus can research Star Citizen's actual visual language before generating the design. The result is a label with authentic Star Citizen aesthetics, not just generic sci-fi.

> "Airi, make that darker and add some Japanese text."

Airi calls `POST /api/v1/creative/refine` with the feedback. If Manus was the original provider, Manus handles the refinement holistically --- darkening both the palette and regenerating artwork with darker tones.

> "Airi, generate disc labels for my last 10 recordings."

Airi iterates over the recording list, calls the appropriate endpoint for each (album or mixtape depending on the source), and calls `POST /api/v1/batch/render` to produce a single sheet of disc face labels.

### 8.5 Why Direct API Calls, Not MCP

The decision to use direct HTTP calls rather than wrapping MDLabel Studio as an MCP tool is deliberate. MCP adds an abstraction layer that is valuable when the tool needs to be discovered and invoked by arbitrary AI agents. In this case, Airi and MDLabel Studio are part of the same ecosystem, deployed together, and the API contract is well-defined. Direct HTTP calls are simpler to debug, easier to test, lower latency, and the OpenAPI spec already provides the machine-readable contract that MCP would otherwise offer. If a future need arises to expose MDLabel Studio to other AI agents outside the Airi ecosystem, an MCP wrapper can be added as a thin layer over the existing API without changing any backend code.

---

## 9. Manus as a User-Facing Creative Tool

Beyond the backend integration described in Section 6.7, Manus also serves as a **user-facing creative tool** for MDLabel Studio. This section describes how a user can point to Manus directly --- not through Airi, but as a standalone creative partner --- for both content and image generation.

### 9.1 The Manus Workflow

When a user selects Manus as their creative provider in the web UI (via the provider selector in Mixtape Mode), the workflow changes subtly but significantly.

Instead of the near-instant structured output from an LLM, the user sees a progress indicator as Manus works through the creative brief. Behind the scenes, Manus is:

1. **Researching** the theme. If the brief mentions "Star Citizen," Manus browses Star Citizen fan art, UI screenshots, and design references to understand the visual language. If the brief mentions "90s TRL," Manus researches the actual TRL set design, the MTV aesthetic of the era, and the graphic design trends of 1999.

2. **Generating the design concept** with full contextual awareness. The color palette is not guessed from training data --- it is informed by actual visual references. The typography recommendations reference real fonts that match the era or theme.

3. **Producing the artwork** as part of the same workflow. Manus generates the cover art image directly, ensuring it is visually coherent with the design concept it just created. The artwork reflects the researched visual language, not just a generic interpretation.

4. **Self-critiquing and refining** before returning the result. Manus can evaluate its own output against the brief and make adjustments before presenting the final concept to the user.

The result is a higher-quality, more contextually aware design concept than any single LLM call can produce --- at the cost of taking 30--120 seconds instead of 1--5 seconds.

### 9.2 When to Use Manus

The web UI provides guidance on when Manus is the right choice:

> **Use Manus when:**
> - Your brief references a specific franchise, game, movie, or cultural aesthetic
> - You want the artwork and design concept to be deeply coherent
> - You are creating a "hero" label that deserves extra creative attention
> - You want research-informed design decisions, not just pattern matching
>
> **Use LLM providers when:**
> - You want fast iteration and quick previews
> - Your brief is simple or uses a theme preset
> - You are batch-generating labels for a collection
> - You are working offline with a local vLLM instance

### 9.3 Manus in the UI

In the Mixtape Mode editor, selecting Manus as the provider changes the AI Concept section to show a richer progress view:

```
+------------------------------------------------------------------+
|  CREATIVE BRIEF    |          LABEL PREVIEW                      |
|  +--------------+  |                                              |
|  | Title:       |  |   +------------------------------------+    |
|  | [Lofi Beats] |  |   | [Spine] [Info] [  Art  ] [Tracklist]|    |
|  | Theme:       |  |   |    |      |      |          |       |    |
|  | [Star Citizen |  |   |    |      | [AI  |          |       |    |
|  |  tech, dark ]|  |   |    |      | art] |          |       |    |
|  |              |  |   +------------------------------------+    |
|  | Provider:    |  |                                              |
|  | [Manus    v] |  |                                              |
|  | [Generate]   |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  MANUS PROGRESS    |                                              |
|  +--------------+  |                                              |
|  | [*] Research |  |                                              |
|  |   Browsing   |  |                                              |
|  |   Star       |  |                                              |
|  |   Citizen    |  |                                              |
|  |   visual     |  |                                              |
|  |   refs...    |  |                                              |
|  | [ ] Concept  |  |                                              |
|  | [ ] Artwork  |  |                                              |
|  | [ ] Review   |  |                                              |
|  +--------------+  |                                              |
+------------------------------------------------------------------+
```

Once Manus completes, the progress section collapses into the standard AI Concept card, and the user can refine from there --- either continuing with Manus for holistic refinement, or switching to an LLM provider for faster iteration.

---

## 10. FOSS Community Access

One of the strongest advantages of the full-stack architecture is that the FastAPI backend is a standalone service that anyone can script against. The FOSS community does not need to use the React web UI at all --- the entire label creation workflow, including Mixtape Mode and Manus integration, is accessible via HTTP.

### 10.1 CLI Scripting Examples

**Create an album label with curl:**

```bash
# Search for an album
curl -s "http://localhost:8000/api/v1/albums/search?q=AAA+10th+Anniversary+Best" | jq '.results[0]'

# Create a label from the album (one-shot)
curl -s -X POST "http://localhost:8000/api/v1/labels/from-album" \
  -H "Content-Type: application/json" \
  -d '{"query": "AAA 10th Anniversary Best", "auto_theme": true}' | jq '.id'

# Render to PDF
curl -s -X POST "http://localhost:8000/api/v1/render/pdf" \
  -H "Content-Type: application/json" \
  -d '{"project_id": "abc123", "options": {"dpi": 300, "bleed_mm": 3, "crop_marks": true}}' \
  -o label.pdf
```

**Create a mixtape label with Manus via curl:**

```bash
# One-shot mixtape label using Manus for content + images
curl -s -X POST "http://localhost:8000/api/v1/mixtape/create" \
  -H "Content-Type: application/json" \
  -d '{
    "creative_brief": "Star Citizen theme, tech-inspired, dark cockpit vibes",
    "title": "Lofi Beats Vol. 4",
    "artist": "Quantum DJ",
    "tracklist": ["1. Quantum Drive", "2. Port Olisar Sunset", "3. Stanton Drift"],
    "generate_artwork": true,
    "provider": "manus"
  }' | jq '.id'
```

**Create a mixtape label with Python (using LLM + separate image gen):**

```python
import requests

# One-shot mixtape label using Claude for concept + DALL-E for artwork
label = requests.post("http://localhost:8000/api/v1/mixtape/create", json={
    "creative_brief": "Japanese city pop revival, neon-lit Tokyo nights, 1983 aesthetic",
    "title": "City Lights シティライツ",
    "artist": "Neon Dreams",
    "tracklist": [
        "1. Plastic Love (Cover)",
        "2. 真夜中のドア",
        "3. Stay With Me",
    ],
    "generate_artwork": True,
    "llm_provider": "openrouter",
    "llm_model": "anthropic/claude-sonnet-4",
    "image_provider": "openai",
}).json()

# Export as PDF
pdf = requests.post("http://localhost:8000/api/v1/render/pdf", json={
    "project_id": label["id"],
    "options": {"dpi": 300, "bleed_mm": 3, "crop_marks": True},
})

with open("city-lights-label.pdf", "wb") as f:
    f.write(pdf.content)
```

### 10.2 Batch Scripting

The API naturally supports batch workflows. A user with a CSV of their MiniDisc collection can write a simple script to generate labels for every disc:

```python
import csv
import requests

BASE = "http://localhost:8000/api/v1"

with open("my_collection.csv") as f:
    reader = csv.DictReader(f)
    project_ids = []
    for row in reader:
        if row["type"] == "album":
            label = requests.post(f"{BASE}/labels/from-album", json={
                "query": f"{row['artist']} {row['album']}",
                "auto_theme": True,
            }).json()
        else:  # mixtape
            label = requests.post(f"{BASE}/mixtape/create", json={
                "creative_brief": row.get("theme", ""),
                "title": row["album"],
                "artist": row["artist"],
                "generate_artwork": True,
                "provider": row.get("provider", "openrouter"),
            }).json()
        project_ids.append(label["id"])

# Batch render all labels into a single PDF
pdf = requests.post(f"{BASE}/batch/render", json={
    "project_ids": project_ids,
    "layout": "sequential",
    "options": {"dpi": 300, "bleed_mm": 3, "crop_marks": True},
})

with open("all_labels.pdf", "wb") as f:
    f.write(pdf.content)
```

### 10.3 Self-Hosting

The Docker Compose setup makes self-hosting straightforward. LLM and Manus configuration are optional --- the application is fully functional for Album Mode without any API keys.

```bash
git clone https://github.com/youruser/mdlabel-studio.git
cd mdlabel-studio

# Minimal setup (Album Mode only, no AI features)
docker compose up -d

# Full setup with AI creative engine
cp .env.example .env
# Edit .env to add your preferred provider keys:
#   OPENROUTER_API_KEY=sk-or-...
#   or OPENAI_API_KEY=sk-...
#   or ANTHROPIC_API_KEY=sk-ant-...
#   or VLLM_BASE_URL=http://your-vllm-server:8000/v1
#   or MANUS_API_KEY=manus-...       # For Manus content + image generation
docker compose up -d

# Web UI at http://localhost:3000
# API at http://localhost:8000
# API docs at http://localhost:8000/docs
```

---

## 11. User Interface Mockup

### 11.1 Album Mode Editor

```
+------------------------------------------------------------------+
|  MDLabel Studio          [Album | Mixtape]    [Save] [Export] [?] |
+------------------------------------------------------------------+
|                    |                                              |
|  ALBUM INFO        |          LABEL PREVIEW                      |
|  +--------------+  |                                              |
|  | [Search...]  |  |   +------------------------------------+    |
|  | Artist: AAA  |  |   | [Spine] [Info] [  Art  ] [Tracklist]|    |
|  | Album: 10th  |  |   |    |      |      |          |       |    |
|  | Year: 2015   |  |   |    |      |      |          |       |    |
|  | Disc: 1 of 3 |  |   |    |      |      |          |       |    |
|  +--------------+  |   +------------------------------------+    |
|                    |                                              |
|  TRACKLIST         |   [Zoom: 100%] [Fit] [Actual Size]          |
|  +--------------+  |   [x] Show bleed  [x] Show crop marks       |
|  | 1. BLOOD on  |  |   [ ] Show safe area                        |
|  | 2. ハリケーン  |  |                                              |
|  | 3. Winter... |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  STYLE             |                                              |
|  +--------------+  |                                              |
|  | BG: [#FF1493]|  |                                              |
|  | Text: [#FFF] |  |                                              |
|  | [Auto Theme] |  |                                              |
|  +--------------+  |                                              |
+------------------------------------------------------------------+
```

### 11.2 Mixtape Mode Editor (with Manus)

```
+------------------------------------------------------------------+
|  MDLabel Studio          [Album | Mixtape]    [Save] [Export] [?] |
+------------------------------------------------------------------+
|                    |                                              |
|  CREATIVE BRIEF    |          LABEL PREVIEW                      |
|  +--------------+  |                                              |
|  | Title:       |  |   +------------------------------------+    |
|  | [Lofi Beats] |  |   | [Spine] [Info] [  Art  ] [Tracklist]|    |
|  | [Vol. 4    ] |  |   |    |      |      |          |       |    |
|  | Theme:       |  |   |    |      | [AI  |          |       |    |
|  | [Star Citizen|  |   |    |      | art] |          |       |    |
|  |  tech, dark ]|  |   +------------------------------------+    |
|  |              |  |                                              |
|  | Provider:    |  |   [Zoom: 100%] [Fit] [Actual Size]          |
|  | [Manus    v] |  |   [x] Show bleed  [x] Show crop marks       |
|  | [Generate]   |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  AI CONCEPT        |                                              |
|  +--------------+  |                                              |
|  | "Quantum     |  |                                              |
|  |  Frequencies |  |                                              |
|  |  from the    |  |                                              |
|  |  Stanton..." |  |                                              |
|  | [Regenerate] |  |                                              |
|  | [Refine...]  |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  ARTWORK           |                                              |
|  +--------------+  |                                              |
|  | [Regenerate] |  |                                              |
|  | [Upload Own] |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  TRACKLIST         |                                              |
|  +--------------+  |                                              |
|  | [Paste/Edit] |  |                                              |
|  +--------------+  |                                              |
|                    |                                              |
|  STYLE (from AI)   |                                              |
|  +--------------+  |                                              |
|  | BG: [#0a0e1a]|  |                                              |
|  | Text:[#e0e8f0]|  |                                              |
|  | Accent:      |  |                                              |
|  |   [#00d4ff]  |  |                                              |
|  | [Override]   |  |                                              |
|  +--------------+  |                                              |
+------------------------------------------------------------------+
```

The left panel scrolls vertically and contains collapsible sections. In Mixtape Mode, the Creative Brief section includes a provider selector dropdown (LLM providers or Manus). The AI Concept and Artwork sections reflect the selected provider's capabilities. The right panel is identical in both modes.

---

## 12. Development Roadmap

The project is structured into six milestones, each delivering a usable increment of functionality. The total timeline is approximately 17 weeks, with Manus integration folded into the AI creative engine milestone.

### Milestone 1: Backend Foundation (Weeks 1--2)

This milestone establishes the FastAPI backend: project scaffolding, Pydantic v2 models for all core data structures (Album, Track, LabelProject, InsertCard, DiscFaceLabel, ColorPalette, RenderOptions), the MusicBrainz metadata service with caching, the color extraction engine, and the basic render engine producing PNG output via Pillow. At the end of this milestone, the API can search for an album, extract colors from cover art, and render a basic label image. Pytest tests cover all services.

### Milestone 2: Frontend Core (Weeks 3--5)

This milestone builds the React frontend: project scaffolding with Vite + React + TypeScript + Tailwind v4, the Konva.js preview canvas, the split-pane editor layout, album search connected to the backend, manual data entry, color picker with auto-theme suggestions from the backend, and basic export (calling the backend render endpoint). At the end of this milestone, a user can search for an album, see a live preview, and export a PNG.

### Milestone 3: Print-Ready Rendering (Weeks 6--8)

This milestone upgrades the backend render engine to produce print-ready PDFs via ReportLab: exact millimeter dimensions, 300 DPI raster embedding, vector crop marks, bleed margins, CMYK color space option, font embedding (including Noto Sans JP for CJK), and the intelligent tracklist layout algorithm. The disc face label renderer and batch grid layout are also implemented. At the end of this milestone, the full export pipeline is production-quality.

### Milestone 4: AI Creative Engine + Manus (Weeks 9--12)

This milestone builds the AI creative engine: LangChain integration with multi-provider support (OpenRouter, Claude, OpenAI, Gemini, vLLM), the design concept generation pipeline with structured Pydantic output, the artwork generation service (DALL-E, Flux, Stable Diffusion), the creative persona system, the theme presets, and the iterative refinement loop. The **Manus agentic integration** is built in parallel: the `manus_provider.py` service, task description templates, progress polling, and the unified content-plus-image workflow. The Mixtape Mode UI components (including the provider selector and Manus progress view) are added to the frontend. At the end of this milestone, users can create AI-assisted mixtape labels through the web UI, Airi, Manus, and the API.

### Milestone 5: Polish & Templates (Weeks 13--15)

This milestone adds multi-disc album support with coordinated color themes, the template system (standard, minimal, retro, cinematic presets), project save/load via the database, spine label designer, the disc face label grid with source/format icons, persona management UI, and UI polish including responsive design. At the end of this milestone, the application is feature-complete for standalone use.

### Milestone 6: Integration & Deployment (Weeks 16--17)

This milestone adds the one-shot endpoints (`/api/v1/labels/from-album` and `/api/v1/mixtape/create`) optimized for Airi and Manus, the Airi creative persona definition, the batch render endpoint, Docker Compose deployment configuration, OpenAPI documentation polish, Spotify API as an alternative metadata source, and community template sharing. At the end of this milestone, the application is production-ready, self-hostable, and integrated with Airi and Manus.

---

## 13. Technical Risks and Mitigations

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| ReportLab font embedding complexity for CJK | Medium | High | Bundle Noto Sans JP as a static asset, pre-register with ReportLab at startup, test with mixed-script labels |
| Frontend-backend preview drift | Medium | Medium | Share dimension constants via a generated config, run visual regression tests comparing frontend preview to backend render |
| MusicBrainz API rate limiting (1 req/sec) | Low | Medium | Implement server-side response caching with TTL, queue concurrent requests |
| LLM output quality variance across providers | Medium | Medium | Use structured output (Pydantic) to enforce schema compliance, provide fallback presets if generation fails, test across all supported providers |
| LLM cost for creative generation | Medium | Low | Default to cost-efficient models (GPT-4.1-mini, Gemini Flash), support vLLM for zero-cost self-hosted inference, cache repeated design concepts |
| Manus API latency for creative workflows | Medium | Medium | Show progress indicators in UI, allow users to switch to LLM providers for faster iteration, implement timeout with graceful fallback |
| Manus API availability/rate limits | Low | Medium | Graceful degradation to LLM providers if Manus is unavailable, cache Manus results for reuse |
| Image generation quality inconsistency | Medium | Medium | Generate multiple variations, let user select or regenerate, support manual artwork upload as fallback |
| Docker image size (fonts + Python deps + LLM libs) | Low | Low | Multi-stage Docker build, only bundle required font weights, make LLM dependencies optional |
| Color extraction producing poor themes | Medium | Low | Provide manual override, curated preset fallbacks, contrast checking with warnings |
| API key management complexity (multiple providers + Manus) | Low | Low | Pydantic Settings with env vars, clear documentation, .env.example template |

---

## 14. Physical Specifications Reference

The following measurements are compiled from the MiniDisc community wiki [12], the DADC MiniDisc Handbook [8], RetroStyleMedia templates [9], and BandCDs production templates [14].

### MiniDisc Case Dimensions

| Component | Dimension | Notes |
|---|---|---|
| Jewel case (outer) | 110mm x 90mm x 14mm | Standard album-style case |
| Jewel case (DADC spec) | 110mm x 91mm x 15mm | Slight variation in published specs |
| Slim case | Varies | Thinner profile, different insert geometry |

### Insert Card Dimensions

| Component | Dimension | Notes |
|---|---|---|
| Cover rollfold insert (5-panel) | Up to 428mm x 105mm | Full-size case, includes all panels |
| Rear tray insert | 106mm x 101.5mm | Goes under the disc tray |
| Spine width | ~5mm | Measured from community templates |
| Bleed margin | 3mm (all sides) | Standard print bleed |

### Disc Face Label Dimensions

| Component | Dimension | Notes |
|---|---|---|
| Label window on disc | 54mm x 37mm | Physical opening on the cartridge |
| Standard sticker label | 52mm x 38mm | Slightly larger than window for coverage |
| Compact sticker label | 50mm x 35mm | Fits easily within window |
| Sony original labels | ~34.5mm x 52.5mm | Supplied with blank MDs |

---

## 15. Conclusion

MDLabel Studio addresses a genuine gap in the MiniDisc community's tooling --- and then goes far beyond it. While existing solutions handle the simple case of disc-face labels, no current tool provides a comprehensive, intelligent workflow for designing the full insert card that makes a MiniDisc collection look professional. And no tool whatsoever addresses the creative challenge of designing original labels for custom mixtapes, compilations, and personal mixes.

The addition of Mixtape Mode and the AI creative engine transforms MDLabel Studio from a utility into a creative tool. It brings back the spirit of hand-crafting mixtape covers --- the personal expression, the thematic coherence, the pride of a well-designed physical artifact --- but accelerates the process with AI that understands design principles, respects print constraints, and can channel a creative persona. When Airi makes a label for her TRL 99 mixtape, it is not a generic template fill --- it is a design that reflects her personality, her aesthetic, and her relationship with the music.

The **Manus integration** elevates the creative engine further by providing a truly agentic creative partner. Where LLM providers offer fast, stateless text generation, Manus offers autonomous, research-informed, multi-step creative workflows that produce deeply coherent results. The ability to point to Manus for both content and images --- in a single workflow where the artwork is informed by the design concept and vice versa --- is a capability that no combination of separate LLM and image generation APIs can replicate. For complex themed briefs like "Star Citizen tech-inspired lofi beats," the difference between a generic sci-fi palette and an authentic Star Citizen aesthetic is the difference between pattern matching and actual research. Manus provides the latter.

The full-stack architecture with a FastAPI backend remains the key architectural decision that unlocks the project's full potential. It ensures that label rendering is consistent and predictable regardless of the client. It provides Airi with a clean, well-documented API to call directly. It gives Manus a structured interface to produce creative output that slots directly into the rendering pipeline. It gives the FOSS community a scriptable service they can integrate into their own workflows. And it provides a natural home for the AI creative engine, where LLM calls, Manus tasks, image generation, and rendering all happen server-side with centralized configuration and caching.

The multi-provider architecture ensures no vendor lock-in. Whether the user prefers OpenRouter for model flexibility, Claude for creative quality, OpenAI for structured output reliability, Gemini for speed, vLLM for privacy and zero cost, or Manus for end-to-end agentic creativity, the same application serves them all.

This is not just a label maker. It is a creative studio for the MiniDisc renaissance.

---

## References

[1]: https://md-label.jkap.io/ "MiniDisc Label Generator by Jae Kaplan"
[2]: https://github.com/RunePML/md-cover-designer "MD Cover Designer by RunePML"
[3]: https://docs.reportlab.com/ "ReportLab Documentation"
[4]: https://musicbrainz.org/doc/MusicBrainz_API "MusicBrainz API Documentation"
[5]: https://coverartarchive.org/ "Cover Art Archive"
[6]: https://www.discogs.com/developers "Discogs API Documentation"
[7]: https://developer.spotify.com/documentation/web-api "Spotify Web API Documentation"
[8]: https://www.minidisc.org/dadc_minidisc_handbook.pdf "DADC MiniDisc Handbook"
[9]: https://www.retrostylemedia.co.uk/product/full-size-minidisc-case-with-a-mint-inner-tray "RetroStyleMedia MiniDisc Case Templates"
[10]: https://www.ebay.com/itm/175847341923 "MiniDisc Label Dimensions (eBay listing)"
[11]: https://www.planetedisque.com/en/music-cassette-minidisc/1318-42-minidisc-labels-50-x-35-mm "Planete Disque MiniDisc Labels"
[12]: https://www.minidisc.wiki/resources/labels "MiniDisc Wiki: Labels Resource"
[13]: https://konvajs.org/docs/shapes/Label.html "Konva.js Label Documentation"
[14]: https://www.bandcds.co.uk/minidisc/minidisc-duplication-and-production/ "BandCDs MiniDisc Production Templates"
[15]: https://python.langchain.com/docs/concepts/chat_models/ "LangChain Chat Models Documentation"
[16]: https://manus.im "Manus AI Platform"
