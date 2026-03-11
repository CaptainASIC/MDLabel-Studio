# MDLabel Studio --- Initial Concept

**Project Codename:** MDLabel Studio
**Version:** 0.4
**Date:** 2026-03-11

---

## 1. Executive Summary

MDLabel Studio is a full-stack web application for designing and printing professional-quality MiniDisc labels. The frontend is built with React, TypeScript, and Tailwind CSS v4, providing an interactive visual editor. The backend is built with FastAPI (Python), serving as the authoritative engine for metadata lookup, image processing, color extraction, label rendering, print-ready PDF generation, and AI-powered creative label design.

The application operates in two distinct modes. **Album Mode** replicates official album releases with auto-imported metadata, cover art, and tracklists. **Mixtape Mode** generates fully original label designs for custom compilations, personal mixes, and curated playlists, powered by a multi-LLM creative engine that produces artwork concepts, thematic color palettes, custom taglines, and stylized layouts from natural language descriptions.

The full-stack architecture serves four distinct audiences through a single codebase. Human users interact through the React web UI. **Airi** calls the FastAPI endpoints directly as part of automated workflows, injecting her own personality and creative preferences into label designs. **Manus** operates as a full agentic creative partner, handling end-to-end label creation with research, content generation, image generation, and iterative refinement in a single autonomous workflow. The FOSS community scripts against the same API using curl, Python, or any HTTP client.

The AI creative engine is built on a **provider-agnostic LLM integration layer** supporting OpenRouter, Claude (Anthropic), OpenAI, Google Gemini, self-hosted vLLM instances, and Manus as a unified content-and-image provider. This ensures no vendor lock-in and allows users to route creative generation through whichever model or agent best suits their needs.

---

## 2. Problem Statement

Creating custom MiniDisc insert cards today requires a multi-step manual workflow: opening a PSD template in Affinity Designer or Photoshop, manually placing album artwork, typing out tracklists, adjusting font sizes, picking colors by hand, aligning elements to the template grid, and exporting at the correct DPI for print. Each new album requires repeating this entire process from scratch.

Existing web-based tools in the MiniDisc community solve only a fraction of this problem. They generate disc-face labels (the small rectangular sticker on the MiniDisc cartridge) but do not address the more complex insert card layout, which includes a spine, an information panel, a main artwork area, and a multi-column tracklist section.

Beyond official album replication, there is an entirely unaddressed use case: **custom mixtape labels**. The MiniDisc format has always been synonymous with personal mix culture --- the 90s and early 2000s tradition of curating playlists, recording them to disc, and creating custom artwork to match. Today's MiniDisc revival community is doing exactly this, but with no tooling purpose-built for the creative side.

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
| No custom mixtape support | No tool helps design original labels for personal mixes or compilations |
| No AI-assisted creativity | No tool leverages LLMs or agentic AI for theme generation or artwork concepts |

---

## 3. Feasibility Assessment

**Verdict: Highly feasible.** Every component maps to mature, well-supported technologies. The full-stack approach simplifies several technical challenges by moving rendering and image processing to the Python backend, where libraries like Pillow, ReportLab, and CairoSVG are more mature than their JavaScript equivalents.

| Capability | Technology | Maturity |
|---|---|---|
| Precise layout rendering at print dimensions | ReportLab (PDF), Pillow (PNG), CairoSVG (SVG) | Production-ready |
| Tracklist auto-import | MusicBrainz API (free, no auth), Discogs, Spotify | Production-ready |
| Color extraction from images | colorthief (Python) | Production-ready |
| Print-ready PDF generation | ReportLab with mm precision, CMYK, crop marks | Production-ready |
| AI-powered creative generation | LangChain (multi-provider), Manus API | Production-ready |
| Interactive canvas preview | Konva.js (react-konva) | Production-ready |

### Comparison with Existing Tools

| Feature | MD Label Generator [1] | MD Cover Designer [2] | MDLabel Studio (Proposed) |
|---|---|---|---|
| Architecture | Client-side only | Client-side (Angular) | Full-stack: React + FastAPI |
| Label Types | Disc face label only | Disc face + spine | Disc face + spine + full insert card + disc label grid |
| Tracklist Support | None | Via Spotify API | MusicBrainz, Discogs, Spotify, manual entry |
| Color Theming | Dark/Light toggle | Manual color selection | Auto-extract from album art + AI-generated themes + manual override |
| CJK Text | Limited | Unknown | First-class support with font selection |
| Multi-disc Albums | No | No | Yes, with disc numbering and color variants |
| Custom Mixtape Labels | No | No | Yes, with AI-assisted creative design |
| AI Art Generation | No | No | Yes, via multi-provider LLM + image gen + Manus |
| Bleed Margins & Crop Marks | No | Cut-line toggle | Full print-ready output with bleed and crop marks |
| Batch Disc Labels | No | No | Grid layout for batch printing disc face labels |
| Export Format | PNG | PNG | PDF (print-ready) + PNG + SVG |
| AI Integration | No | No | Airi + Manus + FOSS scripting |
| Programmatic Access | No | No | Full REST API for scripting |

---

## 4. Product Vision

### 4.1 Two Modes, One Tool

**Album Mode** is the structured workflow for replicating official releases. The user searches for an album, the backend fetches metadata and artwork, and the label is assembled automatically. The creative decisions are minimal: choose a color theme, adjust the layout, and export.

**Mixtape Mode** is the creative workflow for original compositions. The user describes what they want in natural language --- a theme, a mood, a visual reference --- and the AI creative engine generates design suggestions. The user iterates through a conversation between human intent and AI creativity.

Both modes share the same underlying data model (`LabelProject`), the same rendering engine, and the same export pipeline.

### 4.2 Album Mode Workflow

1. **Import.** Search for an album via MusicBrainz/Discogs/Spotify, or enter data manually.
2. **Arrange.** Tracklist auto-arranged into optimal column layout based on track count.
3. **Style.** Color palette auto-extracted from album art with WCAG contrast checking.
4. **Refine.** Adjust any element: reposition artwork, change fonts, edit text, toggle durations.
5. **Export.** Backend generates print-ready PDF at 300 DPI with bleed margins.

### 4.3 Mixtape Mode Workflow

1. **Describe.** Provide a creative brief in natural language (e.g., "Star Citizen theme, tech-inspired, for my lofi beats vol 4").
2. **Generate.** AI creative engine (or Manus) returns a `MixtapeDesignConcept` with title treatment, tagline, color palette, typography, artwork prompt, layout variant, and design rationale.
3. **Artwork.** Generate cover art via DALL-E, Flux, Stable Diffusion, self-hosted ComfyUI, or Manus.
4. **Assemble.** Design concept applied to label template with live preview.
5. **Iterate.** Refine via natural language: "make it darker," "try a more retro feel," "add Japanese text."

### 4.4 Label Types Supported

**Insert Card (Primary Focus).** The folded card inside the MiniDisc jewel case. Based on the MDME 2022 professional templates:

| Component | Dimension | Notes |
|---|---|---|
| Backline card total width | 108.8mm | Rear tray insert |
| Backline card total height | 103.2mm | Rear tray insert |
| Left spine | 11.2mm | Vertical artist/title text |
| Center panel | 86.4mm | Main content area |
| Right spine | 11.2mm | Vertical artist/title text |
| Booklet panel width | 72.7mm | Per panel (rollfold) |
| Booklet panel height | 104.9mm | Per panel (rollfold) |
| Bleed margin | 3mm | All sides |

The user's current workflow uses a 4-panel layout: Spine (~5mm, vertical text) + Info Panel (~65mm, thumbnail + metadata) + Main Art Area (~90mm, large cover art) + Tracklist Area (~160mm, two-column track listing). Total unfolded width approximately 300--335mm, height approximately 100--105mm.

**Disc Face Label.** The rectangular sticker on the MiniDisc cartridge. Label window: 54mm x 37mm. Standard sticker: 52mm x 38mm. Compact sticker: 50mm x 35mm.

**Spine Label.** Narrow strip for shelf identification, typically on Brother TZe-series label tape.

---

## 5. Technical Architecture

### 5.1 Technology Stack

| Layer | Technology | Rationale |
|---|---|---|
| Frontend Framework | React 19 + TypeScript | Component-based, strong typing, large ecosystem |
| Frontend Styling | Tailwind CSS v4 | Utility-first, rapid UI development |
| Frontend Build | Vite | Fast HMR, native TypeScript support |
| Frontend Canvas | Konva.js (react-konva) | Interactive editing, real-time preview |
| Frontend State | Zustand | Lightweight, TypeScript-friendly |
| Backend Framework | FastAPI (Python 3.11+) | Async-first, automatic OpenAPI docs, Pydantic v2 |
| Backend Validation | Pydantic v2 | Data models, request/response schemas, settings |
| LLM Integration | LangChain (chat models) | Provider-agnostic LLM abstraction |
| Agentic Integration | Manus API | Unified content + image generation, research |
| Image Generation | OpenAI Images / Replicate / Manus / Self-hosted | Text-to-image for mixtape artwork |
| PDF Rendering | ReportLab | Precise mm dimensions, CMYK, vector crop marks |
| Image Processing | Pillow + CairoSVG | Server-side manipulation, format conversion |
| Color Extraction | colorthief (Python) | Dominant color extraction from album art |
| Metadata APIs | MusicBrainz + Cover Art Archive | Free, no auth, comprehensive music metadata |
| Database | SQLite (dev) / PostgreSQL (prod) | Project persistence, template storage |
| ORM | SQLAlchemy 2.0 | Async support, type-safe queries |
| Font Rendering | Noto Sans JP, Inter (bundled) | CJK support, consistent rendering |
| Backend Testing | pytest + httpx | Async test client, fixture-based |
| Frontend Testing | Vitest + React Testing Library | Fast unit tests, component testing |
| Deployment | Docker Compose | Single-command deployment |

### 5.2 Backend Structure

```
backend/
  app/
    main.py                       # FastAPI app factory, CORS, lifespan
    config.py                     # Pydantic Settings (env-based config)
    api/
      v1/
        albums.py                 # Album search and detail endpoints
        labels.py                 # Label CRUD endpoints
        render.py                 # PDF/PNG/SVG render endpoints
        templates.py              # Template management endpoints
        colors.py                 # Color extraction endpoint
        batch.py                  # Batch render endpoint
        creative.py               # Creative concept generation/refinement
        mixtape.py                # One-shot mixtape label creation
    models/
      album.py                   # Album, Track, Disc, AlbumSearchResult
      label.py                   # LabelProject, InsertCard, DiscFaceLabel
      template.py                # TemplateDefinition, PanelLayout
      render.py                  # RenderRequest, RenderOptions, ExportFormat
      color.py                   # ColorPalette, ThemeSuggestion
      creative.py                # CreativeBrief, MixtapeDesignConcept, CreativePersona
    services/
      metadata.py                # MusicBrainz, Discogs, Spotify clients
      color_engine.py            # Color extraction + palette generation
      layout_engine.py           # Tracklist arrangement algorithm
      render_engine.py           # ReportLab PDF, Pillow PNG, CairoSVG
      font_manager.py            # Font loading, CJK detection
      image_processor.py         # Album art resizing, caching
      creative_engine.py         # LLM-powered design concept generation
      image_gen.py               # Text-to-image generation
      llm_provider.py            # LangChain provider abstraction
      manus_provider.py          # Manus agentic integration
      persona_manager.py         # Creative persona definitions
    db/
      database.py                # SQLAlchemy async engine + session
      models.py                  # SQLAlchemy ORM models
      migrations/                # Alembic migration scripts
    prompts/
      design_concept.py          # System + user prompt templates
      artwork_prompt.py          # Image generation prompt templates
      refinement.py              # Iterative refinement prompts
      manus_tasks.py             # Manus task descriptions
      personas/                  # Persona definitions (airi, manus, default)
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
    test_api.py                  # Integration tests for API endpoints
  pyproject.toml                 # Dependencies, project metadata
  Dockerfile                     # Backend container definition
```

### 5.3 Frontend Structure

```
frontend/
  src/
    components/
      editor/                    # EditorLayout, LabelPreview, ControlPanel, ModeSelector
      label/                     # InsertCard, DiscFaceLabel, SpineLabel, TracklistLayout
      mixtape/                   # CreativeBriefInput, DesignConceptCard, ArtworkGenerator
      shared/                    # ColorPicker, FontSelector, ImageUploader, ExportDialog
    hooks/                       # useApi, useAlbumSearch, useColorPalette, useCreativeEngine
    stores/                      # labelStore, creativeStore, settingsStore (Zustand)
    types/                       # TypeScript interfaces mirroring Pydantic models
    templates/                   # standard, minimal, retro, cinematic layout definitions
  vite.config.ts
  tailwind.config.ts
  Dockerfile
```

### 5.4 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/v1/albums/search?q={query}` | Search for albums by name/artist |
| `GET` | `/api/v1/albums/{mbid}` | Get full album details by MusicBrainz ID |
| `POST` | `/api/v1/colors/extract` | Extract color palette from uploaded image |
| `POST` | `/api/v1/labels` | Create a new label project |
| `GET` | `/api/v1/labels/{id}` | Retrieve a saved label project |
| `PUT` | `/api/v1/labels/{id}` | Update a label project |
| `POST` | `/api/v1/render/pdf` | Render label to print-ready PDF |
| `POST` | `/api/v1/render/png` | Render label to high-res PNG |
| `POST` | `/api/v1/render/svg` | Render label to SVG |
| `POST` | `/api/v1/batch/render` | Batch render multiple labels to single PDF |
| `GET` | `/api/v1/templates` | List available label templates |
| `POST` | `/api/v1/templates` | Create a custom template |
| `POST` | `/api/v1/labels/from-album` | One-shot: search + auto-theme + create label |
| `POST` | `/api/v1/creative/generate` | Generate design concept from creative brief |
| `POST` | `/api/v1/creative/refine` | Refine existing design concept with feedback |
| `POST` | `/api/v1/creative/artwork` | Generate cover artwork from prompt |
| `POST` | `/api/v1/mixtape/create` | One-shot: brief + generate + artwork + create label |
| `GET` | `/api/v1/personas` | List available creative personas |
| `POST` | `/api/v1/personas` | Create a custom creative persona |

### 5.5 Key Technical Decisions

**Server-side rendering over client-side rendering for export.** All final label rendering happens on the FastAPI backend. This ensures identical output regardless of client (web UI, Airi, Manus, or curl). It eliminates browser-specific font rendering inconsistencies, allows CMYK color space output, and enables batch rendering without a browser instance.

**Pydantic v2 as the shared schema language.** Every data structure is a Pydantic v2 model, providing automatic JSON serialization, request validation, OpenAPI schema generation, and type safety from the API boundary through to the rendering engine.

**LangChain for provider-agnostic LLM access.** The creative engine uses LangChain's chat model abstraction for unified access across OpenRouter, Anthropic, OpenAI, Google, and vLLM. Provider and model are configurable at the application level and overridable per-request.

**Manus as a first-class agentic provider.** Unlike stateless LLM providers, Manus operates as an autonomous agent executing multi-step workflows. The backend delegates (rather than orchestrates) when Manus is selected.

**MusicBrainz as primary metadata source.** Fully open, no authentication required, comprehensive Japanese music coverage. Discogs and Spotify available as alternatives.

**SQLite for development, PostgreSQL for production.** SQLAlchemy 2.0 async support and Alembic migrations make the transition seamless.

---

## 6. AI Creative Engine

### 6.1 Multi-LLM Provider Architecture

| Provider | Models | Use Case |
|---|---|---|
| OpenRouter | Claude, GPT-4.1, Gemini 2.5, Llama, Mixtral, 200+ | Primary routing, model flexibility, cost optimization |
| Anthropic | Claude Opus 4.1, Claude Sonnet | High-quality creative writing, nuanced design direction |
| OpenAI | GPT-5, GPT-4.1, GPT-4.1-mini | Structured output, function calling, DALL-E |
| Google | Gemini 2.5 Flash, Gemini 2.5 Pro | Fast generation, multimodal understanding |
| vLLM | Any self-hosted model | Privacy, no API costs, full control, offline |
| Manus | Agentic (content + images + research) | Unified creative pipeline, end-to-end autonomy |

### 6.2 Design Concept Generation

The creative engine generates a `MixtapeDesignConcept` (Pydantic v2 model) from a natural language creative brief:

| Field | Type | Description |
|---|---|---|
| `title_treatment` | string | Suggested title styling |
| `tagline` | string | Optional subtitle or tagline |
| `color_palette` | ColorPalette | Background, text, accent, secondary colors |
| `typography` | TypographySuggestion | Font family, weight, size recommendations |
| `artwork_prompt` | string | Detailed image generation prompt (1000+ characters) |
| `layout_variant` | string | Template variant (e.g., "minimal-centered", "retro-grid") |
| `mood_tags` | list[str] | Descriptive tags for overall aesthetic |
| `design_rationale` | string | Explanation of design choices |

### 6.3 Creative Personas

Personas are configurable `CreativePersona` models that bias the AI toward specific aesthetics. Airi has her own persona (soft pastels, intentional asymmetry, poetic taglines, Japanese design influence). Users can define their own for consistent series branding.

### 6.4 Theme Presets

| Preset | Description | Color Direction |
|---|---|---|
| Retro 90s | Y2K nostalgia, TRL era | Hot pink, electric blue, silver, lime |
| Lo-Fi Chill | Warm, relaxed, bedroom producer | Warm browns, dusty pink, cream, sage |
| Cyberpunk | Neon-soaked, glitch aesthetic | Neon magenta, cyan, black, chrome |
| Vaporwave | 80s/90s nostalgia, dreamy | Pastel pink, teal, lavender, marble white |
| Minimal | Clean, typographic, Swiss design | Black, white, one accent color |
| Anime/Kawaii | Cute, colorful, Japanese pop | Pastels, bright accents, gradients |
| Dark Academia | Moody, intellectual, classical | Deep burgundy, forest green, gold, cream |
| Space Opera | Cosmic, epic, cinematic sci-fi | Deep navy, nebula purple, star white, gold |

### 6.5 Manus Integration

Manus is architecturally distinct from LLM providers. When selected, Manus autonomously researches the theme, generates the design concept with contextual awareness, produces artwork directly, and self-critiques before returning results. Both content and images come from a single workflow, ensuring coherence between the design concept and the artwork.

| Dimension | LLM Providers | Manus |
|---|---|---|
| Speed | 1--5 seconds | 30--120 seconds |
| Content Quality | High (training data) | Higher for themed briefs (can research) |
| Image Generation | Separate API call, no shared context | Integrated, coherent with concept |
| Iteration | Fast per-step, independent | Slower, but holistic refinement |
| Offline | Yes (vLLM) | No |
| Best For | Quick iterations, simple themes, batch | Complex themes, research-heavy, hero labels |

---

## 7. Integration Points

### 7.1 Airi Integration

Airi calls the FastAPI endpoints directly via HTTP. Key endpoints for Airi:

- `POST /api/v1/labels/from-album` --- one-shot album label creation
- `POST /api/v1/mixtape/create` --- one-shot mixtape label with persona
- `POST /api/v1/creative/refine` --- iterative design refinement
- `POST /api/v1/batch/render` --- batch rendering for collections

Airi can choose between LLM providers for fast iteration or delegate to Manus for deep creative work. The OpenAPI spec serves as the machine-readable contract.

### 7.2 Manus as User-Facing Tool

Manus is selectable in the web UI via the provider dropdown in Mixtape Mode. The UI shows a progress view (Researching, Concept, Artwork, Review) since Manus takes 30--120 seconds.

### 7.3 FOSS Community Access

The entire workflow is accessible via HTTP. Examples include curl for quick labels, Python scripts for batch processing, and CSV-based collection imports. Docker Compose provides single-command self-hosting.

---

## 8. Physical Specifications Reference

### MiniDisc Case Dimensions

| Component | Dimension | Notes |
|---|---|---|
| Jewel case (outer) | 110mm x 90mm x 14mm | Standard album-style case |
| Jewel case (DADC spec) | 110mm x 91mm x 15mm | Slight variation in published specs |

### Insert Card Dimensions (MDME 2022 Templates)

| Component | Dimension | Notes |
|---|---|---|
| Backline card total width | 108.8mm | Rear tray insert |
| Backline card total height | 103.2mm | Rear tray insert |
| Left spine width | 11.2mm | Vertical text |
| Center panel width | 86.4mm | Main content |
| Right spine width | 11.2mm | Vertical text |
| Booklet panel width | 72.7mm | Per panel (rollfold) |
| Booklet panel height | 104.9mm | Per panel (rollfold) |
| Booklet panel count | 6 | Front + 5 fold-out panels |
| Bleed margin | 3mm | All sides |

### Disc Face Label Dimensions

| Component | Dimension | Notes |
|---|---|---|
| Label window on disc | 54mm x 37mm | Physical opening on cartridge |
| Standard sticker label | 52mm x 38mm | Slightly larger for coverage |
| Compact sticker label | 50mm x 35mm | Fits within window |
| Sony original labels | ~34.5mm x 52.5mm | Supplied with blank MDs |

### Disc Label Sheet Layout

| Component | Value | Notes |
|---|---|---|
| Grid layout | 5 columns x 4 rows | 20 labels per sheet |
| Sheet size | A4 (210mm x 297mm) | Standard print size |
| Label content | Artist, art, title, year, genre, source icon | Per label cell |

---

## 9. Development Roadmap

| Milestone | Scope | Timeline |
|---|---|---|
| 1. Backend Foundation | FastAPI scaffolding, Pydantic v2 models, MusicBrainz service, color extraction, basic PNG render, pytest tests | Weeks 1--2 |
| 2. Frontend Core | Vite + React + TypeScript + Tailwind v4, Konva.js preview, editor layout, album search, color picker, basic export | Weeks 3--5 |
| 3. Print-Ready Rendering | ReportLab PDF with mm precision, 300 DPI, crop marks, bleed, CMYK, CJK fonts, tracklist layout algorithm, disc face labels, batch grid | Weeks 6--8 |
| 4. AI Creative Engine + Manus | LangChain multi-provider, design concept generation, artwork generation, personas, theme presets, iterative refinement, Manus agentic integration, Mixtape Mode UI | Weeks 9--12 |
| 5. Polish & Templates | Multi-disc support, template system, project save/load, spine labels, disc label grid, persona management UI, responsive design | Weeks 13--15 |
| 6. Integration & Deployment | One-shot endpoints for Airi/Manus, batch render, Docker Compose, OpenAPI docs, Spotify alternative, community templates | Weeks 16--17 |

---

## 10. References

[1]: https://md-label.jkap.io/ "MiniDisc Label Generator by Jae Kaplan"
[2]: https://github.com/RunePML/md-cover-designer "MD Cover Designer by RunePML"
[3]: https://docs.reportlab.com/ "ReportLab Documentation"
[4]: https://musicbrainz.org/doc/MusicBrainz_API "MusicBrainz API Documentation"
[5]: https://coverartarchive.org/ "Cover Art Archive"
[6]: https://www.discogs.com/developers "Discogs API Documentation"
[7]: https://developer.spotify.com/documentation/web-api "Spotify Web API Documentation"
[8]: https://www.minidisc.org/dadc_minidisc_handbook.pdf "DADC MiniDisc Handbook"
[9]: https://www.retrostylemedia.co.uk/product/full-size-minidisc-case-with-a-mint-inner-tray "RetroStyleMedia MiniDisc Case Templates"
[10]: https://www.minidisc.wiki/resources/labels "MiniDisc Wiki: Labels Resource"
[11]: https://konvajs.org/docs/shapes/Label.html "Konva.js Label Documentation"
[12]: https://www.bandcds.co.uk/minidisc/minidisc-duplication-and-production/ "BandCDs MiniDisc Production Templates"
[13]: https://python.langchain.com/docs/concepts/chat_models/ "LangChain Chat Models Documentation"
[14]: https://manus.im "Manus AI Platform"
