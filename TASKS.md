# MDLabel Studio --- Tasks

**Project:** MDLabel Studio
**Date:** 2026-03-11
**Status:** Planning

---

## Legend

- `[ ]` Not started
- `[~]` In progress
- `[x]` Complete
- `[!]` Blocked

---

## Milestone 1: Backend Foundation (PRP-001) --- Weeks 1--2

### 1.1 Project Scaffolding

- [ ] Initialize Python project with `uv` and `pyproject.toml`
- [ ] Configure FastAPI application factory (`main.py`) with CORS and lifespan
- [ ] Create Pydantic Settings configuration (`config.py`) with environment variable support
- [ ] Set up project directory structure (`app/api/`, `app/models/`, `app/services/`, `app/utils/`, `app/db/`)
- [ ] Create `.env.example` with all configuration variables
- [ ] Set up pytest configuration with `conftest.py` and async test fixtures

### 1.2 Pydantic v2 Data Models

- [ ] Create `models/album.py` --- Album, Track, Disc, AlbumSearchResult
- [ ] Create `models/label.py` --- LabelProject, InsertCard, DiscFaceLabel, SpineLabel
- [ ] Create `models/template.py` --- TemplateDefinition, PanelLayout with MDME 2022 dimensions
- [ ] Create `models/render.py` --- RenderRequest, RenderOptions, ExportFormat
- [ ] Create `models/color.py` --- ColorPalette, ThemeSuggestion
- [ ] Write unit tests for all model validation and serialization

### 1.3 Utility Modules

- [ ] Create `utils/dimensions.py` --- mm-to-px conversion, DPI calculations, MDME 2022 constants
- [ ] Create `utils/contrast.py` --- WCAG contrast ratio calculations, AA/AAA compliance checks
- [ ] Create `utils/cache.py` --- Response caching for API lookups with TTL
- [ ] Write unit tests for dimension conversions and contrast calculations

### 1.4 MusicBrainz Metadata Service

- [ ] Create `services/metadata.py` --- MusicBrainz API client with search and release detail endpoints
- [ ] Implement Cover Art Archive integration for album artwork retrieval
- [ ] Implement server-side response caching with configurable TTL
- [ ] Implement rate limiting (1 req/sec) compliance
- [ ] Create API endpoints: `GET /api/v1/albums/search`, `GET /api/v1/albums/{mbid}`
- [ ] Write unit tests with mocked API responses

### 1.5 Color Extraction Engine

- [ ] Create `services/color_engine.py` --- colorthief integration for dominant color extraction
- [ ] Implement palette generation with WCAG contrast checking (background, text, accent)
- [ ] Create API endpoint: `POST /api/v1/colors/extract`
- [ ] Write unit tests with sample album art images

### 1.6 Basic Render Engine

- [ ] Create `services/render_engine.py` --- Pillow-based PNG rendering at correct dimensions
- [ ] Implement insert card layout rendering (spine + info + art + tracklist panels)
- [ ] Create `services/font_manager.py` --- Font loading with Noto Sans JP and Inter
- [ ] Create `services/image_processor.py` --- Album art resizing and caching
- [ ] Create API endpoints: `POST /api/v1/labels`, `POST /api/v1/render/png`
- [ ] Write unit tests verifying output dimensions and content placement

### 1.7 Integration Testing

- [ ] Write integration tests for full workflow: search album, extract colors, create label, render PNG
- [ ] Verify all API endpoints return correct status codes and response schemas
- [ ] Verify Pydantic model serialization round-trips correctly

---

## Milestone 2: Frontend Core (PRP-002) --- Weeks 3--5

### 2.1 Project Scaffolding

- [ ] Initialize Vite + React + TypeScript project
- [ ] Configure Tailwind CSS v4
- [ ] Install and configure Konva.js (react-konva)
- [ ] Set up Zustand stores
- [ ] Create TypeScript interfaces mirroring backend Pydantic models

### 2.2 Editor Layout

- [ ] Build split-pane editor layout (controls left, preview right)
- [ ] Implement Album Mode / Mixtape Mode toggle
- [ ] Build collapsible control panel sections

### 2.3 Album Search & Import

- [ ] Build album search component connected to backend API
- [ ] Build search results display with album art thumbnails
- [ ] Build manual data entry form (artist, album, year, tracklist)
- [ ] Implement tracklist auto-parsing (numbered, timed, plain formats)

### 2.4 Label Preview Canvas

- [ ] Build Konva.js canvas with insert card template at correct proportions
- [ ] Implement real-time preview updating from control panel changes
- [ ] Build zoom controls (fit, actual size, percentage)
- [ ] Implement bleed margin and crop mark overlay toggles

### 2.5 Color & Style Controls

- [ ] Build color picker component with hex input
- [ ] Connect auto-theme button to backend color extraction endpoint
- [ ] Implement contrast warning display
- [ ] Build font selector component

### 2.6 Basic Export

- [ ] Build export dialog connected to backend render endpoints
- [ ] Implement PNG download
- [ ] Write component tests with Vitest + React Testing Library

---

## Milestone 3: Print-Ready Rendering (PRP-003) --- Weeks 6--8

### 3.1 ReportLab PDF Engine

- [ ] Upgrade render engine to produce PDF via ReportLab
- [ ] Implement exact mm dimensions for all label types
- [ ] Implement 300 DPI raster embedding
- [ ] Implement vector crop marks at corners and midpoints
- [ ] Implement configurable bleed margins (default 3mm)
- [ ] Implement CMYK color space option

### 3.2 Font Embedding

- [ ] Bundle Noto Sans JP font weights as static assets
- [ ] Pre-register fonts with ReportLab at startup
- [ ] Implement CJK text detection and automatic font switching
- [ ] Test mixed-script labels (Latin + Japanese + Korean)

### 3.3 Tracklist Layout Algorithm

- [ ] Create `services/layout_engine.py` with intelligent column splitting
- [ ] Implement track count-based layout strategy (1--6 single, 7--12 two-col, 13+ compact)
- [ ] Implement CJK character width accounting
- [ ] Implement track duration right-alignment with fixed spacing
- [ ] Implement romanized/English translation display alongside original titles
- [ ] Write unit tests for all layout strategies and edge cases

### 3.4 Disc Face Labels

- [ ] Implement disc face label renderer (52mm x 38mm)
- [ ] Implement batch grid layout (5x4, 20-up on A4)
- [ ] Implement source/format icons (CD Copy, Vinyl Copy, Streaming, MDLP, Hi-MD)
- [ ] Create API endpoint: `POST /api/v1/batch/render`

### 3.5 SVG Export

- [ ] Implement SVG export via CairoSVG
- [ ] Implement font outlining for vector compatibility
- [ ] Create API endpoint: `POST /api/v1/render/svg`

### 3.6 Frontend PDF Export

- [ ] Connect frontend export dialog to PDF render endpoint
- [ ] Add PDF export options (DPI, bleed, crop marks, CMYK toggle)
- [ ] Implement download progress indicator

---

## Milestone 4: AI Creative Engine + Manus (PRP-004) --- Weeks 9--12

### 4.1 LLM Provider Integration

- [ ] Create `services/llm_provider.py` --- LangChain chat model abstraction
- [ ] Implement OpenRouter provider configuration
- [ ] Implement Anthropic (Claude) provider configuration
- [ ] Implement OpenAI provider configuration
- [ ] Implement Google (Gemini) provider configuration
- [ ] Implement vLLM (self-hosted) provider configuration
- [ ] Implement per-request provider override

### 4.2 Design Concept Generation

- [ ] Create `models/creative.py` --- CreativeBrief, MixtapeDesignConcept, CreativePersona
- [ ] Create `services/creative_engine.py` --- Design concept generation with structured Pydantic output
- [ ] Create `prompts/design_concept.py` --- System and user prompt templates
- [ ] Create API endpoint: `POST /api/v1/creative/generate`
- [ ] Write unit tests with mocked LLM responses

### 4.3 Artwork Generation

- [ ] Create `services/image_gen.py` --- Text-to-image generation service
- [ ] Implement OpenAI DALL-E provider
- [ ] Implement Replicate/Flux provider
- [ ] Implement self-hosted ComfyUI provider
- [ ] Create API endpoint: `POST /api/v1/creative/artwork`

### 4.4 Creative Personas

- [ ] Create `services/persona_manager.py` --- Persona CRUD and injection
- [ ] Create `prompts/personas/airi.py` --- Airi persona definition
- [ ] Create `prompts/personas/default.py` --- Default persona
- [ ] Create API endpoints: `GET /api/v1/personas`, `POST /api/v1/personas`

### 4.5 Theme Presets

- [ ] Implement 8 curated theme presets (Retro 90s, Lo-Fi Chill, Cyberpunk, Vaporwave, Minimal, Anime/Kawaii, Dark Academia, Space Opera)
- [ ] Create API endpoint for preset listing and selection

### 4.6 Iterative Refinement

- [ ] Create `prompts/refinement.py` --- Refinement prompt templates
- [ ] Implement refinement history with version tracking
- [ ] Create API endpoint: `POST /api/v1/creative/refine`

### 4.7 Manus Integration

- [ ] Create `services/manus_provider.py` --- Manus agentic integration implementing CreativeProvider interface
- [ ] Create `prompts/manus_tasks.py` --- Manus task description templates
- [ ] Implement progress polling for Manus workflows
- [ ] Implement unified content + image generation workflow
- [ ] Write integration tests with mocked Manus responses

### 4.8 One-Shot Mixtape Endpoint

- [ ] Create API endpoint: `POST /api/v1/mixtape/create` --- brief + generate + artwork + create label
- [ ] Implement provider selection (LLM vs. Manus) per-request
- [ ] Write integration tests for full mixtape workflow

### 4.9 Mixtape Mode UI

- [ ] Build creative brief input component
- [ ] Build provider selector dropdown (LLM providers + Manus)
- [ ] Build design concept card with regenerate/refine controls
- [ ] Build artwork generator component with upload fallback
- [ ] Build Manus progress view (Research, Concept, Artwork, Review)
- [ ] Build theme preset selector
- [ ] Connect all components to backend API

---

## Milestone 5: Polish & Templates (PRP-005) --- Weeks 13--15

### 5.1 Multi-Disc Album Support

- [ ] Implement linked label project sets for multi-disc releases
- [ ] Implement coordinated color themes across disc set
- [ ] Implement disc numbering and subtitle management

### 5.2 Template System

- [ ] Create template definitions (standard, minimal, retro, cinematic)
- [ ] Implement template selection and customization
- [ ] Create API endpoints: `GET /api/v1/templates`, `POST /api/v1/templates`

### 5.3 Project Persistence

- [ ] Set up SQLAlchemy 2.0 ORM models
- [ ] Set up Alembic migrations
- [ ] Implement project save/load via database
- [ ] Create API endpoints: `GET /api/v1/labels/{id}`, `PUT /api/v1/labels/{id}`

### 5.4 Spine Label Designer

- [ ] Implement spine label renderer for Brother TZe-series tape
- [ ] Add spine label controls to frontend

### 5.5 Disc Face Label Grid

- [ ] Implement disc label grid populated from saved projects
- [ ] Implement source/format icon selection
- [ ] Add grid management UI to frontend

### 5.6 Persona Management UI

- [ ] Build persona creation/editing form in frontend
- [ ] Build persona selection in Mixtape Mode

### 5.7 UI Polish

- [ ] Implement responsive design for tablet use
- [ ] Polish all control panel sections
- [ ] Implement keyboard shortcuts for common actions

---

## Milestone 6: Integration & Deployment (PRP-006) --- Weeks 16--17

### 6.1 One-Shot Album Endpoint

- [ ] Create API endpoint: `POST /api/v1/labels/from-album` --- search + auto-theme + create label
- [ ] Optimize for Airi and scripting workflows

### 6.2 Batch Render

- [ ] Implement batch render for multiple labels to single PDF
- [ ] Support sequential and grid layouts
- [ ] Create API endpoint: `POST /api/v1/batch/render`

### 6.3 Docker Compose Deployment

- [ ] Create backend Dockerfile (multi-stage build)
- [ ] Create frontend Dockerfile (Nginx serving built React app)
- [ ] Create `docker-compose.yml` (frontend + backend + PostgreSQL)
- [ ] Create `.env.example` with all provider keys documented
- [ ] Test full deployment from clean state

### 6.4 Documentation

- [ ] Polish OpenAPI documentation with examples
- [ ] Write README.md with quickstart guide
- [ ] Write FOSS scripting examples (curl, Python, batch CSV)
- [ ] Document Airi integration patterns

### 6.5 Alternative Metadata Sources

- [ ] Implement Spotify API as alternative metadata source
- [ ] Implement Discogs API as alternative metadata source

### 6.6 Community Features

- [ ] Implement template sharing (export/import)
- [ ] Implement persona sharing (export/import)
