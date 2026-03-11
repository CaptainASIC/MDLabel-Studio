# MDLabel Studio --- Planning

**Project:** MDLabel Studio
**Date:** 2026-03-11
**Status:** Planning

---

## 1. High-Level Direction

MDLabel Studio is a full-stack web application for designing and printing professional-quality MiniDisc labels. It replaces the current manual PSD template workflow (Affinity Designer / Photoshop) with an intelligent, API-driven tool that serves four audiences: human users via a web UI, Airi via direct REST API calls, Manus as an agentic creative partner, and the FOSS community via scripting.

The application operates in two modes. **Album Mode** replicates official album releases with auto-imported metadata, cover art, and tracklists. **Mixtape Mode** generates fully original label designs for custom compilations using a multi-LLM AI creative engine that produces artwork concepts, thematic color palettes, custom taglines, and stylized layouts from natural language descriptions.

---

## 2. Scope

### In Scope

The following capabilities are within the scope of this project:

| Area | Description |
|---|---|
| Insert card design | Full 4-panel layout (spine + info + art + tracklist) with exact MDME 2022 dimensions |
| Backline card design | Rear tray insert (108.8mm x 103.2mm) with dual spines |
| Disc face labels | Cartridge sticker labels (52mm x 38mm) with batch grid (5x4, 20-up) |
| Spine labels | Narrow strip labels for shelf identification |
| Album metadata import | MusicBrainz (primary), Discogs, Spotify as alternatives |
| Color theming | Auto-extract from album art, AI-generated, preset, manual |
| Tracklist auto-layout | Intelligent column splitting based on track count and title length |
| CJK text support | First-class Japanese/Korean/Chinese text rendering with Noto Sans JP |
| Print-ready export | PDF (300 DPI, mm precision, crop marks, bleed, CMYK option), PNG, SVG |
| Multi-disc albums | Linked label sets with coordinated color themes |
| AI creative engine | Multi-LLM design concept generation via LangChain |
| AI artwork generation | DALL-E, Flux, Stable Diffusion, ComfyUI, Manus |
| Creative personas | Configurable style profiles (Airi persona, user-defined personas) |
| Theme presets | 8 curated starting points (Retro 90s, Lo-Fi Chill, Cyberpunk, etc.) |
| Manus integration | Agentic creative partner for unified content + image generation |
| Airi integration | Direct REST API calls for automated label workflows |
| FOSS scripting | Full API access via curl, Python, any HTTP client |
| Self-hosting | Docker Compose single-command deployment |

### Out of Scope (for now)

| Area | Rationale |
|---|---|
| Mobile app | Web-first; responsive design covers tablet use |
| User accounts / multi-tenancy | Single-user or household deployment initially |
| E-commerce / label printing service | Focus on self-printing; commercial printing is a future consideration |
| MCP server wrapper | Direct API is simpler; MCP can be added later if needed |
| Cassette / CD label support | MiniDisc-specific initially; extensible architecture allows future formats |

---

## 3. Technical Architecture

### 3.1 Stack Summary

| Layer | Technology | Version |
|---|---|---|
| Frontend | React + TypeScript + Tailwind CSS v4 | React 19, TS 5.x, TW 4.x |
| Frontend Build | Vite | 6.x |
| Frontend Canvas | Konva.js (react-konva) | Latest |
| Frontend State | Zustand | Latest |
| Backend | FastAPI (Python) | 0.115+ |
| Backend Validation | Pydantic v2 | 2.x (plan for v3) |
| LLM Integration | LangChain | Latest |
| PDF Rendering | ReportLab | Latest |
| Image Processing | Pillow + CairoSVG | Latest |
| Color Extraction | colorthief | Latest |
| Database | SQLite (dev) / PostgreSQL (prod) | SQLAlchemy 2.0 + Alembic |
| Font Stack | Noto Sans JP, Inter (bundled) | Latest |
| Testing | pytest + httpx (backend), Vitest (frontend) | Latest |
| Deployment | Docker Compose | Latest |

### 3.2 Architecture Pattern

The application follows a **client-server architecture** where the FastAPI backend is the authoritative engine for all label rendering, metadata lookup, color extraction, and AI creative generation. The React frontend is a presentation layer that provides interactive editing and live preview via Konva.js, but all export and rendering is delegated to the backend.

This ensures identical output regardless of client (web UI, Airi, Manus, or curl script).

### 3.3 Key Technical Decisions

| Decision | Rationale |
|---|---|
| Server-side rendering for export | Consistent output across all clients, CMYK support, no browser dependency |
| Pydantic v2 as shared schema | Automatic validation, OpenAPI generation, type safety end-to-end |
| LangChain for LLM abstraction | Provider-agnostic, structured output, unified interface |
| Manus as first-class provider | Agentic workflow for unified content + image, not just another LLM |
| MusicBrainz as primary metadata | Free, no auth, comprehensive Japanese music coverage |
| SQLite for dev, PostgreSQL for prod | Zero-config local dev, production-ready with Alembic migrations |
| Files under 500 lines | Split into modules when approaching limit |
| Test early and often | Every new function gets unit tests |
| Documentation as code develops | Comments and docs written alongside implementation |

### 3.4 Physical Dimensions (MDME 2022 Templates)

| Component | Width | Height | Notes |
|---|---|---|---|
| Backline card (total) | 108.8mm | 103.2mm | Rear tray insert |
| Backline left spine | 11.2mm | 103.2mm | Vertical text |
| Backline center panel | 86.4mm | 103.2mm | Main content |
| Backline right spine | 11.2mm | 103.2mm | Vertical text |
| Booklet panel | 72.7mm | 104.9mm | Per panel (rollfold) |
| Disc face label (standard) | 52mm | 38mm | Cartridge sticker |
| Disc face label (compact) | 50mm | 35mm | Fits within window |
| Bleed margin | 3mm | 3mm | All sides |

### 3.5 AI Provider Matrix

| Provider | Content | Images | Research | Offline | Speed |
|---|---|---|---|---|---|
| OpenRouter | Yes | No (route to image gen) | No | No | Fast |
| Anthropic (Claude) | Yes | No | No | No | Fast |
| OpenAI | Yes | Yes (DALL-E) | No | No | Fast |
| Google (Gemini) | Yes | No | No | No | Fast |
| vLLM (self-hosted) | Yes | No | No | Yes | Fast |
| Manus | Yes | Yes | Yes | No | 30--120s |

---

## 4. Development Approach

### 4.1 Milestone Structure

Development is organized into 6 milestones, each delivering a usable increment. Each milestone will have its own PRP document with step-by-step implementation instructions for Warp.

| Milestone | PRP | Scope | Timeline |
|---|---|---|---|
| 1. Backend Foundation | PRP-001 | FastAPI scaffolding, Pydantic v2 models, MusicBrainz service, color extraction, basic PNG render, pytest | Weeks 1--2 |
| 2. Frontend Core | PRP-002 | Vite + React + TS + TW4, Konva.js preview, editor layout, album search, color picker, basic export | Weeks 3--5 |
| 3. Print-Ready Rendering | PRP-003 | ReportLab PDF, mm precision, 300 DPI, crop marks, bleed, CMYK, CJK fonts, tracklist layout, disc labels, batch grid | Weeks 6--8 |
| 4. AI Creative Engine + Manus | PRP-004 | LangChain multi-provider, design concepts, artwork gen, personas, themes, refinement, Manus integration, Mixtape Mode UI | Weeks 9--12 |
| 5. Polish & Templates | PRP-005 | Multi-disc, template system, save/load, spine labels, disc grid, persona UI, responsive | Weeks 13--15 |
| 6. Integration & Deployment | PRP-006 | One-shot endpoints, batch render, Docker Compose, OpenAPI docs, Spotify, community templates | Weeks 16--17 |

### 4.2 Testing Strategy

Every milestone includes comprehensive tests. The backend uses pytest with httpx async test client and fixture-based test data. The frontend uses Vitest with React Testing Library. Visual regression tests compare frontend preview to backend render output.

### 4.3 Documentation Strategy

Documentation is written alongside code. Every module includes docstrings. Every Pydantic model includes field descriptions. The OpenAPI spec is auto-generated and serves as the API contract for Airi, Manus, and FOSS scripting.

---

## 5. Dependencies and Prerequisites

### 5.1 External Services

| Service | Required | Auth | Notes |
|---|---|---|---|
| MusicBrainz API | Yes (Album Mode) | None | 1 req/sec rate limit, cached server-side |
| Cover Art Archive | Yes (Album Mode) | None | Linked from MusicBrainz |
| OpenRouter / Claude / OpenAI / Gemini | Optional (Mixtape Mode) | API key | Any one provider sufficient |
| vLLM | Optional (Mixtape Mode) | Base URL | Self-hosted, no API costs |
| Manus API | Optional (Mixtape Mode) | API key | Agentic content + image |
| DALL-E / Flux / Replicate | Optional (artwork gen) | API key | Or self-hosted ComfyUI |

### 5.2 Bundled Assets

| Asset | Purpose | License |
|---|---|---|
| Noto Sans JP | CJK text rendering | OFL (SIL Open Font License) |
| Inter | Latin text rendering | OFL |
| Montserrat | Template compatibility (MDME templates) | OFL |

### 5.3 Development Tools

| Tool | Purpose |
|---|---|
| uv | Python package management |
| pnpm | Node.js package management |
| Docker + Docker Compose | Containerized deployment |
| Alembic | Database migrations |

---

## 6. Risk Summary

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| ReportLab CJK font embedding | Medium | High | Bundle Noto Sans JP, pre-register at startup, test mixed-script |
| Frontend-backend preview drift | Medium | Medium | Shared dimension constants, visual regression tests |
| MusicBrainz rate limiting | Low | Medium | Server-side caching with TTL, request queuing |
| LLM output quality variance | Medium | Medium | Pydantic structured output, fallback presets |
| Manus API latency | Medium | Medium | Progress indicators, fallback to LLM providers |
| Docker image size | Low | Low | Multi-stage build, optional LLM deps |

---

## 7. References

See `INITIAL.md` for the full reference list.
