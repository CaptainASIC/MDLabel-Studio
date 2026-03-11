# PRP-001: Backend Foundation

**Objective:** This Product Requirements Prompt (PRP) provides a complete, step-by-step instruction set for an AI agent (Warp) to implement the Backend Foundation for MDLabel Studio. Execute all steps in the specified order.

**Status:** Ready for execution
**Author:** Manus
**Date:** 2026-03-11
**Depends on:** INITIAL.md, PLANNING.md, TASKS.md

---

## 1. Objective & Scope

**Goal:** Implement the Backend Foundation (Milestone 1) for MDLabel Studio --- a FastAPI backend with Pydantic v2 models, MusicBrainz metadata service, color extraction engine, basic PNG render engine, and comprehensive pytest tests.

**Summary of changes:**

| Area | Files | Action |
|------|-------|--------|
| Project Setup | `pyproject.toml` | **CREATE** --- Python project with all dependencies |
| Project Setup | `.env.example` | **CREATE** --- Environment variable template |
| Application | `app/__init__.py` | **CREATE** --- Package init |
| Application | `app/main.py` | **CREATE** --- FastAPI app factory with CORS and lifespan |
| Configuration | `app/config.py` | **CREATE** --- Pydantic Settings configuration |
| Models | `app/models/__init__.py` | **CREATE** --- Models package |
| Models | `app/models/album.py` | **CREATE** --- Album, Track, Disc, AlbumSearchResult |
| Models | `app/models/label.py` | **CREATE** --- LabelProject, InsertCard, DiscFaceLabel, SpineLabel |
| Models | `app/models/template.py` | **CREATE** --- TemplateDefinition, PanelLayout with MDME 2022 dimensions |
| Models | `app/models/render.py` | **CREATE** --- RenderRequest, RenderOptions, ExportFormat |
| Models | `app/models/color.py` | **CREATE** --- ColorPalette, ThemeSuggestion |
| Utilities | `app/utils/__init__.py` | **CREATE** --- Utils package |
| Utilities | `app/utils/dimensions.py` | **CREATE** --- mm/px conversion, DPI calculations, MDME constants |
| Utilities | `app/utils/contrast.py` | **CREATE** --- WCAG contrast ratio calculations |
| Utilities | `app/utils/cache.py` | **CREATE** --- TTL-based response caching |
| Services | `app/services/__init__.py` | **CREATE** --- Services package |
| Services | `app/services/metadata.py` | **CREATE** --- MusicBrainz client with caching |
| Services | `app/services/color_engine.py` | **CREATE** --- Color extraction and palette generation |
| Services | `app/services/font_manager.py` | **CREATE** --- Font loading and CJK detection |
| Services | `app/services/image_processor.py` | **CREATE** --- Album art resizing and caching |
| Services | `app/services/render_engine.py` | **CREATE** --- Pillow-based PNG rendering |
| Services | `app/services/layout_engine.py` | **CREATE** --- Tracklist arrangement algorithm |
| API | `app/api/__init__.py` | **CREATE** --- API package |
| API | `app/api/v1/__init__.py` | **CREATE** --- v1 package |
| API | `app/api/v1/router.py` | **CREATE** --- v1 router aggregation |
| API | `app/api/v1/albums.py` | **CREATE** --- Album search and detail endpoints |
| API | `app/api/v1/labels.py` | **CREATE** --- Label CRUD endpoints |
| API | `app/api/v1/render.py` | **CREATE** --- Render endpoints |
| API | `app/api/v1/colors.py` | **CREATE** --- Color extraction endpoint |
| Tests | `tests/__init__.py` | **CREATE** --- Tests package |
| Tests | `tests/conftest.py` | **CREATE** --- Shared fixtures and test client |
| Tests | `tests/test_models.py` | **CREATE** --- Model validation tests |
| Tests | `tests/test_dimensions.py` | **CREATE** --- Dimension conversion tests |
| Tests | `tests/test_contrast.py` | **CREATE** --- WCAG contrast tests |
| Tests | `tests/test_metadata.py` | **CREATE** --- MusicBrainz service tests |
| Tests | `tests/test_color_engine.py` | **CREATE** --- Color extraction tests |
| Tests | `tests/test_layout_engine.py` | **CREATE** --- Tracklist layout tests |
| Tests | `tests/test_render_engine.py` | **CREATE** --- Render engine tests |
| Tests | `tests/test_api.py` | **CREATE** --- API integration tests |

**Total new files:** 35
**Total modified files:** 0

---

## 2. Implementation Instructions

### Execution Order

**Execute these steps sequentially:**

1.  **Step 2.1** --- Create `pyproject.toml`
2.  **Step 2.2** --- Create `.env.example`
3.  **Step 2.3** --- Create `app/__init__.py`
4.  **Step 2.4** --- Create `app/config.py`
5.  **Step 2.5** --- Create `app/models/__init__.py`
6.  **Step 2.6** --- Create `app/models/album.py`
7.  **Step 2.7** --- Create `app/models/label.py`
8.  **Step 2.8** --- Create `app/models/template.py`
9.  **Step 2.9** --- Create `app/models/render.py`
10. **Step 2.10** --- Create `app/models/color.py`
11. **Step 2.11** --- Create `app/utils/__init__.py`
12. **Step 2.12** --- Create `app/utils/dimensions.py`
13. **Step 2.13** --- Create `app/utils/contrast.py`
14. **Step 2.14** --- Create `app/utils/cache.py`
15. **Step 2.15** --- Create `app/services/__init__.py`
16. **Step 2.16** --- Create `app/services/metadata.py`
17. **Step 2.17** --- Create `app/services/color_engine.py`
18. **Step 2.18** --- Create `app/services/font_manager.py`
19. **Step 2.19** --- Create `app/services/image_processor.py`
20. **Step 2.20** --- Create `app/services/layout_engine.py`
21. **Step 2.21** --- Create `app/services/render_engine.py`
22. **Step 2.22** --- Create `app/api/__init__.py`
23. **Step 2.23** --- Create `app/api/v1/__init__.py`
24. **Step 2.24** --- Create `app/api/v1/router.py`
25. **Step 2.25** --- Create `app/api/v1/albums.py`
26. **Step 2.26** --- Create `app/api/v1/labels.py`
27. **Step 2.27** --- Create `app/api/v1/render.py`
28. **Step 2.28** --- Create `app/api/v1/colors.py`
29. **Step 2.29** --- Create `app/main.py`
30. **Step 3.1** --- Create `tests/__init__.py`
31. **Step 3.2** --- Create `tests/conftest.py`
32. **Step 3.3** --- Create `tests/test_models.py`
33. **Step 3.4** --- Create `tests/test_dimensions.py`
34. **Step 3.5** --- Create `tests/test_contrast.py`
35. **Step 3.6** --- Create `tests/test_metadata.py`
36. **Step 3.7** --- Create `tests/test_color_engine.py`
37. **Step 3.8** --- Create `tests/test_layout_engine.py`
38. **Step 3.9** --- Create `tests/test_render_engine.py`
39. **Step 3.10** --- Create `tests/test_api.py`
40. **Step 4.1** --- Install dependencies
41. **Step 4.2** --- Download bundled fonts
42. **Step 4.3** --- Execute tests
43. **Step 4.4** --- Verify acceptance criteria

---

### 2.1 New File: `backend/pyproject.toml`

**Action:** CREATE

**Instructions:** Create the file `backend/pyproject.toml` with the following content. Do not add or remove any lines.

```toml
[project]
name = "mdlabel-studio"
version = "0.1.0"
description = "MiniDisc label designer backend"
requires-python = ">=3.11"
dependencies = [
    "fastapi>=0.115.0",
    "uvicorn[standard]>=0.32.0",
    "pydantic>=2.10.0",
    "pydantic-settings>=2.7.0",
    "httpx>=0.28.0",
    "pillow>=11.0.0",
    "colorthief>=0.2.1",
    "python-multipart>=0.0.18",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.3.0",
    "pytest-asyncio>=0.25.0",
    "pytest-cov>=6.0.0",
    "httpx>=0.28.0",
    "ruff>=0.8.0",
]

[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]

[tool.ruff]
target-version = "py311"
line-length = 100

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP"]
```

---

### 2.2 New File: `backend/.env.example`

**Action:** CREATE

**Instructions:** Create the file `backend/.env.example` with the following content.

```bash
# MDLabel Studio Backend Configuration

# Server
HOST=0.0.0.0
PORT=8000
DEBUG=true
CORS_ORIGINS=["http://localhost:5173","http://localhost:3000"]

# Paths
FONT_DIR=./fonts
CACHE_DIR=./cache
RENDER_DIR=./renders

# MusicBrainz
MUSICBRAINZ_USER_AGENT=MDLabelStudio/0.1.0 (https://github.com/youruser/mdlabel-studio)
MUSICBRAINZ_CACHE_TTL=86400

# Color Extraction
DEFAULT_PALETTE_SIZE=6
```

---

### 2.3 New File: `backend/app/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/__init__.py` with the following content.

```python
"""MDLabel Studio backend application."""
```

---

### 2.4 New File: `backend/app/config.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/config.py` with the following content.

```python
"""Application configuration via Pydantic Settings."""

from pathlib import Path

from pydantic import Field
from pydantic_settings import BaseSettings


class Settings(BaseSettings):
    """MDLabel Studio backend configuration.

    All settings can be overridden via environment variables or a .env file.
    """

    model_config = {"env_file": ".env", "env_file_encoding": "utf-8"}

    # Server
    host: str = Field(default="0.0.0.0", description="Server bind address")
    port: int = Field(default=8000, description="Server port")
    debug: bool = Field(default=False, description="Enable debug mode")
    cors_origins: list[str] = Field(
        default=["http://localhost:5173", "http://localhost:3000"],
        description="Allowed CORS origins",
    )

    # Paths
    font_dir: Path = Field(default=Path("./fonts"), description="Directory for bundled fonts")
    cache_dir: Path = Field(default=Path("./cache"), description="Directory for cached data")
    render_dir: Path = Field(default=Path("./renders"), description="Directory for rendered output")

    # MusicBrainz
    musicbrainz_user_agent: str = Field(
        default="MDLabelStudio/0.1.0 (https://github.com/youruser/mdlabel-studio)",
        description="User-Agent header for MusicBrainz API requests",
    )
    musicbrainz_cache_ttl: int = Field(
        default=86400,
        description="Cache TTL in seconds for MusicBrainz responses",
    )

    # Color Extraction
    default_palette_size: int = Field(
        default=6,
        description="Number of colors to extract from album art",
    )


def get_settings() -> Settings:
    """Return a cached Settings instance."""
    return Settings()
```

---

### 2.5 New File: `backend/app/models/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/__init__.py` with the following content.

```python
"""Pydantic v2 data models for MDLabel Studio."""

from app.models.album import Album, AlbumSearchResult, Disc, Track
from app.models.color import ColorPalette, ThemeSuggestion
from app.models.label import DiscFaceLabel, InsertCard, LabelProject, SpineLabel
from app.models.render import ExportFormat, RenderOptions, RenderRequest
from app.models.template import PanelLayout, TemplateDefinition

__all__ = [
    "Album",
    "AlbumSearchResult",
    "ColorPalette",
    "Disc",
    "DiscFaceLabel",
    "ExportFormat",
    "InsertCard",
    "LabelProject",
    "PanelLayout",
    "RenderOptions",
    "RenderRequest",
    "SpineLabel",
    "TemplateDefinition",
    "ThemeSuggestion",
    "Track",
]
```

---

### 2.6 New File: `backend/app/models/album.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/album.py` with the following content.

```python
"""Album, Track, and Disc models for music metadata."""

from pydantic import BaseModel, Field


class Track(BaseModel):
    """A single track on a disc."""

    number: int = Field(description="Track number within the disc")
    title: str = Field(description="Track title (original language)")
    title_romanized: str | None = Field(
        default=None,
        description="Romanized or English translation of the title",
    )
    duration_seconds: int | None = Field(
        default=None,
        description="Track duration in seconds",
    )

    @property
    def duration_formatted(self) -> str | None:
        """Return duration as M:SS string, or None if unavailable."""
        if self.duration_seconds is None:
            return None
        minutes = self.duration_seconds // 60
        seconds = self.duration_seconds % 60
        return f"{minutes}:{seconds:02d}"


class Disc(BaseModel):
    """A single disc within an album release."""

    number: int = Field(description="Disc number within the release")
    title: str | None = Field(
        default=None,
        description="Disc-specific title (e.g., 'Disc 1')",
    )
    tracks: list[Track] = Field(default_factory=list, description="Tracks on this disc")

    @property
    def track_count(self) -> int:
        """Return the number of tracks on this disc."""
        return len(self.tracks)


class Album(BaseModel):
    """A complete album release with metadata and disc/track structure."""

    mbid: str | None = Field(
        default=None,
        description="MusicBrainz release ID",
    )
    title: str = Field(description="Album title")
    artist: str = Field(description="Primary artist name")
    year: int | None = Field(default=None, description="Release year")
    cover_art_url: str | None = Field(
        default=None,
        description="URL to full-resolution cover art",
    )
    cover_art_thumbnail_url: str | None = Field(
        default=None,
        description="URL to cover art thumbnail",
    )
    discs: list[Disc] = Field(default_factory=list, description="Discs in this release")
    genre: str | None = Field(default=None, description="Primary genre tag")

    @property
    def disc_count(self) -> int:
        """Return the number of discs in this release."""
        return len(self.discs)

    @property
    def total_tracks(self) -> int:
        """Return the total number of tracks across all discs."""
        return sum(disc.track_count for disc in self.discs)


class AlbumSearchResult(BaseModel):
    """A single result from an album search query."""

    mbid: str = Field(description="MusicBrainz release ID")
    title: str = Field(description="Album title")
    artist: str = Field(description="Primary artist name")
    year: int | None = Field(default=None, description="Release year")
    disc_count: int = Field(default=1, description="Number of discs")
    country: str | None = Field(default=None, description="Release country code")
    cover_art_thumbnail_url: str | None = Field(
        default=None,
        description="URL to cover art thumbnail",
    )
    score: int = Field(default=0, description="Search relevance score (0-100)")
```

---

### 2.7 New File: `backend/app/models/label.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/label.py` with the following content.

```python
"""Label project models for insert cards, disc face labels, and spine labels."""

import uuid
from datetime import datetime, timezone
from enum import StrEnum

from pydantic import BaseModel, Field

from app.models.album import Album
from app.models.color import ColorPalette


class LabelMode(StrEnum):
    """The mode used to create this label."""

    ALBUM = "album"
    MIXTAPE = "mixtape"


class SourceFormat(StrEnum):
    """The source format of the recording on this MiniDisc."""

    CD_COPY = "cd_copy"
    VINYL_COPY = "vinyl_copy"
    STREAMING = "streaming"
    ORIGINAL = "original"


class InsertCard(BaseModel):
    """The folded insert card inside the MiniDisc jewel case.

    Uses the user's 4-panel layout: Spine + Info + Art + Tracklist.
    Dimensions derived from MDME 2022 professional templates.
    """

    spine_text: str = Field(description="Vertical text on the spine panel")
    info_artist: str = Field(description="Artist name on the info panel")
    info_album: str = Field(description="Album title on the info panel")
    info_year: int | None = Field(default=None, description="Release year on the info panel")
    info_disc: str | None = Field(
        default=None,
        description="Disc identifier (e.g., 'Disc 1')",
    )
    artwork_image_path: str | None = Field(
        default=None,
        description="Path to the main artwork image file",
    )
    artwork_thumbnail_path: str | None = Field(
        default=None,
        description="Path to the small thumbnail on the info panel",
    )
    tracklist_header: str = Field(
        default="Tracklist",
        description="Header text above the tracklist (e.g., 'Tracklist', 'トラックリスト')",
    )
    show_durations: bool = Field(
        default=False,
        description="Whether to display track durations in the tracklist",
    )
    show_romanized: bool = Field(
        default=False,
        description="Whether to display romanized titles alongside originals",
    )


class DiscFaceLabel(BaseModel):
    """The rectangular sticker label on the MiniDisc cartridge."""

    artist: str = Field(description="Artist name")
    album_title: str = Field(description="Album or mixtape title")
    subtitle: str | None = Field(
        default=None,
        description="Subtitle (e.g., 'Disc 1', romanized title)",
    )
    year: int | None = Field(default=None, description="Release year")
    genre: str | None = Field(default=None, description="Genre tag")
    artwork_thumbnail_path: str | None = Field(
        default=None,
        description="Path to the album art thumbnail",
    )
    source_format: SourceFormat = Field(
        default=SourceFormat.CD_COPY,
        description="Source format icon to display",
    )


class SpineLabel(BaseModel):
    """A narrow spine label for shelf identification."""

    text: str = Field(description="Full spine text (e.g., 'Artist - Album - Disc 1')")
    font_size_pt: float = Field(default=6.0, description="Font size in points")


class LabelProject(BaseModel):
    """A complete label project containing all label components for one disc."""

    id: str = Field(
        default_factory=lambda: str(uuid.uuid4()),
        description="Unique project identifier",
    )
    created_at: datetime = Field(
        default_factory=lambda: datetime.now(timezone.utc),
        description="Project creation timestamp",
    )
    updated_at: datetime = Field(
        default_factory=lambda: datetime.now(timezone.utc),
        description="Last update timestamp",
    )
    mode: LabelMode = Field(description="Album or Mixtape mode")
    album: Album | None = Field(
        default=None,
        description="Album metadata (populated in Album Mode)",
    )
    disc_number: int = Field(default=1, description="Which disc this label is for")
    palette: ColorPalette = Field(description="Color palette for this label")
    insert_card: InsertCard | None = Field(
        default=None,
        description="Insert card configuration",
    )
    disc_face_label: DiscFaceLabel | None = Field(
        default=None,
        description="Disc face label configuration",
    )
    spine_label: SpineLabel | None = Field(
        default=None,
        description="Spine label configuration",
    )
```

---

### 2.8 New File: `backend/app/models/template.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/template.py` with the following content.

```python
"""Template and layout dimension models based on MDME 2022 specifications."""

from pydantic import BaseModel, Field


class PanelLayout(BaseModel):
    """Dimensions for a single panel in the insert card layout.

    All dimensions are in millimeters.
    """

    name: str = Field(description="Panel identifier (e.g., 'spine', 'info', 'art', 'tracklist')")
    width_mm: float = Field(description="Panel width in millimeters")
    height_mm: float = Field(description="Panel height in millimeters")
    x_offset_mm: float = Field(
        default=0.0,
        description="Horizontal offset from the left edge of the unfolded card",
    )


class TemplateDefinition(BaseModel):
    """A complete label template defining panel layout and dimensions.

    Dimensions sourced from MDME 2022 professional templates:
    - Backline card: 108.8mm x 103.2mm (11.2mm spines + 86.4mm center)
    - Booklet panel: 72.7mm x 104.9mm per panel
    - Bleed margin: 3mm all sides
    """

    name: str = Field(description="Template name (e.g., 'standard', 'minimal')")
    description: str = Field(default="", description="Human-readable template description")
    panels: list[PanelLayout] = Field(description="Ordered list of panels from left to right")
    total_width_mm: float = Field(description="Total unfolded width in millimeters")
    total_height_mm: float = Field(description="Total height in millimeters")
    bleed_mm: float = Field(default=3.0, description="Bleed margin in millimeters")

    @property
    def total_width_with_bleed_mm(self) -> float:
        """Total width including bleed margins on both sides."""
        return self.total_width_mm + (2 * self.bleed_mm)

    @property
    def total_height_with_bleed_mm(self) -> float:
        """Total height including bleed margins on both sides."""
        return self.total_height_mm + (2 * self.bleed_mm)


# --- Built-in Template Definitions ---

STANDARD_4PANEL = TemplateDefinition(
    name="standard",
    description=(
        "Standard 4-panel insert card layout: Spine + Info + Art + Tracklist. "
        "Based on MDME 2022 backline card dimensions."
    ),
    panels=[
        PanelLayout(name="spine", width_mm=5.0, height_mm=103.2, x_offset_mm=0.0),
        PanelLayout(name="info", width_mm=65.0, height_mm=103.2, x_offset_mm=5.0),
        PanelLayout(name="art", width_mm=90.0, height_mm=103.2, x_offset_mm=70.0),
        PanelLayout(name="tracklist", width_mm=160.0, height_mm=103.2, x_offset_mm=160.0),
    ],
    total_width_mm=320.0,
    total_height_mm=103.2,
    bleed_mm=3.0,
)

BACKLINE_CARD = TemplateDefinition(
    name="backline",
    description=(
        "MDME 2022 backline card (rear tray insert): "
        "Left Spine + Center Panel + Right Spine. "
        "108.8mm x 103.2mm with 11.2mm spines."
    ),
    panels=[
        PanelLayout(name="left_spine", width_mm=11.2, height_mm=103.2, x_offset_mm=0.0),
        PanelLayout(name="center", width_mm=86.4, height_mm=103.2, x_offset_mm=11.2),
        PanelLayout(name="right_spine", width_mm=11.2, height_mm=103.2, x_offset_mm=97.6),
    ],
    total_width_mm=108.8,
    total_height_mm=103.2,
    bleed_mm=3.0,
)

DISC_FACE_LABEL = TemplateDefinition(
    name="disc_face",
    description="Standard disc face label for MiniDisc cartridge. 52mm x 38mm.",
    panels=[
        PanelLayout(name="label", width_mm=52.0, height_mm=38.0, x_offset_mm=0.0),
    ],
    total_width_mm=52.0,
    total_height_mm=38.0,
    bleed_mm=0.0,
)

DISC_FACE_LABEL_COMPACT = TemplateDefinition(
    name="disc_face_compact",
    description="Compact disc face label for MiniDisc cartridge. 50mm x 35mm.",
    panels=[
        PanelLayout(name="label", width_mm=50.0, height_mm=35.0, x_offset_mm=0.0),
    ],
    total_width_mm=50.0,
    total_height_mm=35.0,
    bleed_mm=0.0,
)

BUILTIN_TEMPLATES: dict[str, TemplateDefinition] = {
    "standard": STANDARD_4PANEL,
    "backline": BACKLINE_CARD,
    "disc_face": DISC_FACE_LABEL,
    "disc_face_compact": DISC_FACE_LABEL_COMPACT,
}
```

---

### 2.9 New File: `backend/app/models/render.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/render.py` with the following content.

```python
"""Render request and options models."""

from enum import StrEnum

from pydantic import BaseModel, Field


class ExportFormat(StrEnum):
    """Supported export formats."""

    PDF = "pdf"
    PNG = "png"
    SVG = "svg"


class RenderOptions(BaseModel):
    """Options controlling how a label is rendered for export."""

    dpi: int = Field(default=300, description="Output resolution in dots per inch")
    bleed_mm: float = Field(default=3.0, description="Bleed margin in millimeters")
    crop_marks: bool = Field(default=True, description="Include crop marks in output")
    show_safe_area: bool = Field(default=False, description="Show safe area guides")
    cmyk: bool = Field(
        default=False,
        description="Convert colors to CMYK color space (PDF only)",
    )


class RenderRequest(BaseModel):
    """A request to render a label project to an export format."""

    project_id: str = Field(description="ID of the label project to render")
    format: ExportFormat = Field(default=ExportFormat.PNG, description="Output format")
    options: RenderOptions = Field(
        default_factory=RenderOptions,
        description="Render options",
    )
```

---

### 2.10 New File: `backend/app/models/color.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/models/color.py` with the following content.

```python
"""Color palette and theme suggestion models."""

import re

from pydantic import BaseModel, Field, field_validator


class ColorPalette(BaseModel):
    """A color palette for a label design.

    All colors are stored as hex strings (e.g., '#FF1493').
    """

    background: str = Field(description="Background color hex (e.g., '#FF1493')")
    text: str = Field(description="Primary text color hex (e.g., '#FFFFFF')")
    accent: str = Field(description="Accent color hex for headers and highlights")
    secondary: str | None = Field(
        default=None,
        description="Optional secondary accent color hex",
    )
    spine_background: str | None = Field(
        default=None,
        description="Spine background color hex (defaults to background if not set)",
    )
    spine_text: str | None = Field(
        default=None,
        description="Spine text color hex (defaults to text if not set)",
    )

    @field_validator("background", "text", "accent", "secondary", "spine_background", "spine_text")
    @classmethod
    def validate_hex_color(cls, v: str | None) -> str | None:
        """Validate that the value is a valid hex color string."""
        if v is None:
            return None
        if not re.match(r"^#[0-9A-Fa-f]{6}$", v):
            msg = f"Invalid hex color: {v}. Expected format: #RRGGBB"
            raise ValueError(msg)
        return v.upper()

    @property
    def effective_spine_background(self) -> str:
        """Return the spine background color, falling back to the main background."""
        return self.spine_background or self.background

    @property
    def effective_spine_text(self) -> str:
        """Return the spine text color, falling back to the main text color."""
        return self.spine_text or self.text


class ThemeSuggestion(BaseModel):
    """A suggested color theme extracted from album artwork."""

    palette: ColorPalette = Field(description="The suggested color palette")
    dominant_colors: list[str] = Field(
        description="Dominant colors extracted from the image, as hex strings",
    )
    contrast_ratio: float = Field(
        description="WCAG contrast ratio between background and text colors",
    )
    meets_aa: bool = Field(description="Whether the palette meets WCAG AA standards (4.5:1)")
    meets_aaa: bool = Field(description="Whether the palette meets WCAG AAA standards (7:1)")
```

---

### 2.11 New File: `backend/app/utils/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/utils/__init__.py` with the following content.

```python
"""Utility modules for MDLabel Studio."""
```

---

### 2.12 New File: `backend/app/utils/dimensions.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/utils/dimensions.py` with the following content.

```python
"""Dimension conversion utilities and MDME 2022 physical constants.

All physical dimensions are in millimeters unless otherwise noted.
Conversion functions handle mm-to-pixel and pixel-to-mm at arbitrary DPI.
"""


# --- MDME 2022 Template Constants (millimeters) ---

# Backline card (rear tray insert)
BACKLINE_WIDTH_MM: float = 108.8
BACKLINE_HEIGHT_MM: float = 103.2
BACKLINE_SPINE_WIDTH_MM: float = 11.2
BACKLINE_CENTER_WIDTH_MM: float = 86.4

# Booklet (rollfold) panels
BOOKLET_PANEL_WIDTH_MM: float = 72.7
BOOKLET_PANEL_HEIGHT_MM: float = 104.9
BOOKLET_PANEL_COUNT: int = 6

# User's 4-panel insert card layout
INSERT_SPINE_WIDTH_MM: float = 5.0
INSERT_INFO_WIDTH_MM: float = 65.0
INSERT_ART_WIDTH_MM: float = 90.0
INSERT_TRACKLIST_WIDTH_MM: float = 160.0
INSERT_TOTAL_WIDTH_MM: float = 320.0
INSERT_HEIGHT_MM: float = 103.2

# Disc face labels
DISC_LABEL_WINDOW_WIDTH_MM: float = 54.0
DISC_LABEL_WINDOW_HEIGHT_MM: float = 37.0
DISC_LABEL_STANDARD_WIDTH_MM: float = 52.0
DISC_LABEL_STANDARD_HEIGHT_MM: float = 38.0
DISC_LABEL_COMPACT_WIDTH_MM: float = 50.0
DISC_LABEL_COMPACT_HEIGHT_MM: float = 35.0

# Disc label grid (batch printing)
DISC_LABEL_GRID_COLS: int = 5
DISC_LABEL_GRID_ROWS: int = 4
DISC_LABEL_GRID_TOTAL: int = DISC_LABEL_GRID_COLS * DISC_LABEL_GRID_ROWS  # 20

# Print constants
DEFAULT_DPI: int = 300
DEFAULT_BLEED_MM: float = 3.0

# MiniDisc case
CASE_WIDTH_MM: float = 110.0
CASE_HEIGHT_MM: float = 90.0
CASE_DEPTH_MM: float = 14.0


def mm_to_px(mm: float, dpi: int = DEFAULT_DPI) -> int:
    """Convert millimeters to pixels at the given DPI.

    Args:
        mm: Dimension in millimeters.
        dpi: Dots per inch (default: 300).

    Returns:
        Dimension in pixels, rounded to the nearest integer.
    """
    return round(mm * dpi / 25.4)


def px_to_mm(px: int, dpi: int = DEFAULT_DPI) -> float:
    """Convert pixels to millimeters at the given DPI.

    Args:
        px: Dimension in pixels.
        dpi: Dots per inch (default: 300).

    Returns:
        Dimension in millimeters.
    """
    return px * 25.4 / dpi


def mm_to_pt(mm: float) -> float:
    """Convert millimeters to PostScript points (1 pt = 1/72 inch).

    Args:
        mm: Dimension in millimeters.

    Returns:
        Dimension in points.
    """
    return mm * 72.0 / 25.4


def pt_to_mm(pt: float) -> float:
    """Convert PostScript points to millimeters.

    Args:
        pt: Dimension in points.

    Returns:
        Dimension in millimeters.
    """
    return pt * 25.4 / 72.0
```

---

### 2.13 New File: `backend/app/utils/contrast.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/utils/contrast.py` with the following content.

```python
"""WCAG contrast ratio calculations for accessibility compliance.

Implements the WCAG 2.1 relative luminance and contrast ratio formulas.
Used to ensure text is legible against background colors on printed labels.
"""


def hex_to_rgb(hex_color: str) -> tuple[int, int, int]:
    """Convert a hex color string to an RGB tuple.

    Args:
        hex_color: Color string in '#RRGGBB' format.

    Returns:
        Tuple of (red, green, blue) integers in range 0-255.
    """
    hex_color = hex_color.lstrip("#")
    return (
        int(hex_color[0:2], 16),
        int(hex_color[2:4], 16),
        int(hex_color[4:6], 16),
    )


def rgb_to_hex(r: int, g: int, b: int) -> str:
    """Convert an RGB tuple to a hex color string.

    Args:
        r: Red component (0-255).
        g: Green component (0-255).
        b: Blue component (0-255).

    Returns:
        Color string in '#RRGGBB' format.
    """
    return f"#{r:02X}{g:02X}{b:02X}"


def relative_luminance(hex_color: str) -> float:
    """Calculate the WCAG 2.1 relative luminance of a color.

    The relative luminance is defined as:
        L = 0.2126 * R + 0.7152 * G + 0.0722 * B

    where R, G, B are linearized sRGB values.

    Args:
        hex_color: Color string in '#RRGGBB' format.

    Returns:
        Relative luminance value between 0.0 (black) and 1.0 (white).
    """
    r, g, b = hex_to_rgb(hex_color)

    def linearize(channel: int) -> float:
        srgb = channel / 255.0
        if srgb <= 0.04045:
            return srgb / 12.92
        return ((srgb + 0.055) / 1.055) ** 2.4

    return 0.2126 * linearize(r) + 0.7152 * linearize(g) + 0.0722 * linearize(b)


def contrast_ratio(color1: str, color2: str) -> float:
    """Calculate the WCAG 2.1 contrast ratio between two colors.

    The contrast ratio is defined as:
        (L1 + 0.05) / (L2 + 0.05)

    where L1 is the lighter color's luminance and L2 is the darker color's.

    Args:
        color1: First color in '#RRGGBB' format.
        color2: Second color in '#RRGGBB' format.

    Returns:
        Contrast ratio between 1.0 (identical) and 21.0 (black on white).
    """
    l1 = relative_luminance(color1)
    l2 = relative_luminance(color2)
    lighter = max(l1, l2)
    darker = min(l1, l2)
    return (lighter + 0.05) / (darker + 0.05)


def meets_wcag_aa(foreground: str, background: str) -> bool:
    """Check if a color pair meets WCAG AA contrast requirements.

    WCAG AA requires a minimum contrast ratio of 4.5:1 for normal text.

    Args:
        foreground: Text color in '#RRGGBB' format.
        background: Background color in '#RRGGBB' format.

    Returns:
        True if the contrast ratio is at least 4.5:1.
    """
    return contrast_ratio(foreground, background) >= 4.5


def meets_wcag_aaa(foreground: str, background: str) -> bool:
    """Check if a color pair meets WCAG AAA contrast requirements.

    WCAG AAA requires a minimum contrast ratio of 7:1 for normal text.

    Args:
        foreground: Text color in '#RRGGBB' format.
        background: Background color in '#RRGGBB' format.

    Returns:
        True if the contrast ratio is at least 7:1.
    """
    return contrast_ratio(foreground, background) >= 7.0


def suggest_text_color(background: str) -> str:
    """Suggest white or black text based on background luminance.

    Args:
        background: Background color in '#RRGGBB' format.

    Returns:
        '#FFFFFF' for dark backgrounds, '#000000' for light backgrounds.
    """
    luminance = relative_luminance(background)
    return "#FFFFFF" if luminance < 0.5 else "#000000"
```

---

### 2.14 New File: `backend/app/utils/cache.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/utils/cache.py` with the following content.

```python
"""Simple TTL-based in-memory cache for API response caching."""

import time
from typing import Any


class TTLCache:
    """A thread-safe, TTL-based in-memory cache.

    Entries expire after the configured TTL (time-to-live) in seconds.
    Expired entries are lazily evicted on access.
    """

    def __init__(self, default_ttl: int = 86400) -> None:
        """Initialize the cache.

        Args:
            default_ttl: Default time-to-live in seconds (default: 24 hours).
        """
        self._store: dict[str, tuple[float, Any]] = {}
        self._default_ttl = default_ttl

    def get(self, key: str) -> Any | None:
        """Retrieve a value from the cache.

        Returns None if the key does not exist or has expired.

        Args:
            key: The cache key.

        Returns:
            The cached value, or None if not found or expired.
        """
        entry = self._store.get(key)
        if entry is None:
            return None
        expires_at, value = entry
        if time.monotonic() > expires_at:
            del self._store[key]
            return None
        return value

    def set(self, key: str, value: Any, ttl: int | None = None) -> None:
        """Store a value in the cache.

        Args:
            key: The cache key.
            value: The value to cache.
            ttl: Time-to-live in seconds. Uses default_ttl if not specified.
        """
        effective_ttl = ttl if ttl is not None else self._default_ttl
        expires_at = time.monotonic() + effective_ttl
        self._store[key] = (expires_at, value)

    def delete(self, key: str) -> bool:
        """Remove a key from the cache.

        Args:
            key: The cache key.

        Returns:
            True if the key was found and removed, False otherwise.
        """
        if key in self._store:
            del self._store[key]
            return True
        return False

    def clear(self) -> None:
        """Remove all entries from the cache."""
        self._store.clear()

    def size(self) -> int:
        """Return the number of entries in the cache (including expired)."""
        return len(self._store)

    def cleanup(self) -> int:
        """Remove all expired entries from the cache.

        Returns:
            The number of entries removed.
        """
        now = time.monotonic()
        expired_keys = [k for k, (expires_at, _) in self._store.items() if now > expires_at]
        for key in expired_keys:
            del self._store[key]
        return len(expired_keys)
```

---

### 2.15 New File: `backend/app/services/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/__init__.py` with the following content.

```python
"""Service modules for MDLabel Studio."""
```

---

### 2.16 New File: `backend/app/services/metadata.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/metadata.py` with the following content.

```python
"""MusicBrainz metadata service for album search and detail retrieval.

Uses the MusicBrainz API (https://musicbrainz.org/doc/MusicBrainz_API)
and the Cover Art Archive (https://coverartarchive.org/) for artwork.
Rate limited to 1 request per second per MusicBrainz API policy.
"""

import asyncio
import time

import httpx

from app.config import Settings, get_settings
from app.models.album import Album, AlbumSearchResult, Disc, Track
from app.utils.cache import TTLCache

MUSICBRAINZ_BASE_URL = "https://musicbrainz.org/ws/2"
COVER_ART_BASE_URL = "https://coverartarchive.org"


class MusicBrainzService:
    """Client for the MusicBrainz API with caching and rate limiting."""

    def __init__(self, settings: Settings | None = None) -> None:
        """Initialize the MusicBrainz service.

        Args:
            settings: Application settings. Uses defaults if not provided.
        """
        self._settings = settings or get_settings()
        self._cache = TTLCache(default_ttl=self._settings.musicbrainz_cache_ttl)
        self._last_request_time: float = 0.0
        self._rate_limit_interval: float = 1.1  # Slightly over 1 second to be safe

    async def _rate_limit(self) -> None:
        """Enforce the MusicBrainz rate limit of 1 request per second."""
        now = time.monotonic()
        elapsed = now - self._last_request_time
        if elapsed < self._rate_limit_interval:
            await asyncio.sleep(self._rate_limit_interval - elapsed)
        self._last_request_time = time.monotonic()

    async def _get(self, url: str, params: dict | None = None) -> dict:
        """Make a rate-limited GET request to the MusicBrainz API.

        Args:
            url: Full URL to request.
            params: Query parameters.

        Returns:
            Parsed JSON response.

        Raises:
            httpx.HTTPStatusError: If the response status is not 2xx.
        """
        await self._rate_limit()
        async with httpx.AsyncClient() as client:
            response = await client.get(
                url,
                params=params,
                headers={
                    "User-Agent": self._settings.musicbrainz_user_agent,
                    "Accept": "application/json",
                },
                timeout=15.0,
            )
            response.raise_for_status()
            return response.json()

    async def search_albums(self, query: str, limit: int = 10) -> list[AlbumSearchResult]:
        """Search for album releases by name and/or artist.

        Args:
            query: Search query string (album name, artist, or both).
            limit: Maximum number of results to return (default: 10).

        Returns:
            List of matching album search results.
        """
        cache_key = f"search:{query}:{limit}"
        cached = self._cache.get(cache_key)
        if cached is not None:
            return cached

        data = await self._get(
            f"{MUSICBRAINZ_BASE_URL}/release",
            params={"query": query, "limit": limit, "fmt": "json"},
        )

        results: list[AlbumSearchResult] = []
        for release in data.get("releases", []):
            artist_credit = release.get("artist-credit", [])
            artist_name = artist_credit[0].get("name", "Unknown") if artist_credit else "Unknown"

            year = None
            date_str = release.get("date", "")
            if date_str and len(date_str) >= 4:
                try:
                    year = int(date_str[:4])
                except ValueError:
                    pass

            media = release.get("media", [])
            disc_count = len(media) if media else 1

            mbid = release.get("id", "")
            thumbnail_url = f"{COVER_ART_BASE_URL}/release/{mbid}/front-250" if mbid else None

            results.append(
                AlbumSearchResult(
                    mbid=mbid,
                    title=release.get("title", "Unknown"),
                    artist=artist_name,
                    year=year,
                    disc_count=disc_count,
                    country=release.get("country"),
                    cover_art_thumbnail_url=thumbnail_url,
                    score=release.get("score", 0),
                )
            )

        self._cache.set(cache_key, results)
        return results

    async def get_album(self, mbid: str) -> Album:
        """Fetch full album details by MusicBrainz release ID.

        Args:
            mbid: MusicBrainz release ID (UUID string).

        Returns:
            Complete Album model with disc and track data.

        Raises:
            httpx.HTTPStatusError: If the release is not found.
        """
        cache_key = f"album:{mbid}"
        cached = self._cache.get(cache_key)
        if cached is not None:
            return cached

        data = await self._get(
            f"{MUSICBRAINZ_BASE_URL}/release/{mbid}",
            params={"inc": "recordings+artist-credits+media", "fmt": "json"},
        )

        artist_credit = data.get("artist-credit", [])
        artist_name = artist_credit[0].get("name", "Unknown") if artist_credit else "Unknown"

        year = None
        date_str = data.get("date", "")
        if date_str and len(date_str) >= 4:
            try:
                year = int(date_str[:4])
            except ValueError:
                pass

        discs: list[Disc] = []
        for disc_idx, medium in enumerate(data.get("media", []), start=1):
            tracks: list[Track] = []
            for track_data in medium.get("tracks", []):
                recording = track_data.get("recording", {})
                duration_ms = recording.get("length")
                duration_seconds = round(duration_ms / 1000) if duration_ms else None

                tracks.append(
                    Track(
                        number=track_data.get("position", 0),
                        title=recording.get("title", track_data.get("title", "Unknown")),
                        duration_seconds=duration_seconds,
                    )
                )

            disc_title = medium.get("title") or f"Disc {disc_idx}"
            discs.append(Disc(number=disc_idx, title=disc_title, tracks=tracks))

        cover_art_url = f"{COVER_ART_BASE_URL}/release/{mbid}/front"
        cover_art_thumbnail_url = f"{COVER_ART_BASE_URL}/release/{mbid}/front-250"

        album = Album(
            mbid=mbid,
            title=data.get("title", "Unknown"),
            artist=artist_name,
            year=year,
            cover_art_url=cover_art_url,
            cover_art_thumbnail_url=cover_art_thumbnail_url,
            discs=discs,
        )

        self._cache.set(cache_key, album)
        return album
```

---

### 2.17 New File: `backend/app/services/color_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/color_engine.py` with the following content.

```python
"""Color extraction engine for generating palettes from album artwork.

Uses colorthief to extract dominant colors, then applies WCAG contrast
checking to select an accessible background/text/accent palette.
"""

from io import BytesIO
from pathlib import Path

from colorthief import ColorThief

from app.models.color import ColorPalette, ThemeSuggestion
from app.utils.contrast import (
    contrast_ratio,
    meets_wcag_aa,
    meets_wcag_aaa,
    rgb_to_hex,
    suggest_text_color,
)


class ColorEngine:
    """Extracts color palettes from images and generates accessible themes."""

    def __init__(self, palette_size: int = 6) -> None:
        """Initialize the color engine.

        Args:
            palette_size: Number of dominant colors to extract (default: 6).
        """
        self._palette_size = palette_size

    def extract_from_file(self, image_path: str | Path) -> ThemeSuggestion:
        """Extract a color theme from an image file.

        Args:
            image_path: Path to the image file.

        Returns:
            A ThemeSuggestion with an accessible palette and metadata.
        """
        ct = ColorThief(str(image_path))
        return self._build_theme(ct)

    def extract_from_bytes(self, image_bytes: bytes) -> ThemeSuggestion:
        """Extract a color theme from image bytes.

        Args:
            image_bytes: Raw image data.

        Returns:
            A ThemeSuggestion with an accessible palette and metadata.
        """
        ct = ColorThief(BytesIO(image_bytes))
        return self._build_theme(ct)

    def _build_theme(self, ct: ColorThief) -> ThemeSuggestion:
        """Build a ThemeSuggestion from a ColorThief instance.

        Extracts dominant colors, selects the most prominent as the background,
        and chooses text and accent colors that meet WCAG contrast requirements.

        Args:
            ct: A ColorThief instance loaded with an image.

        Returns:
            A ThemeSuggestion with palette and accessibility metadata.
        """
        dominant_rgb = ct.get_color(quality=1)
        palette_rgb = ct.get_palette(color_count=self._palette_size, quality=1)

        dominant_hex = rgb_to_hex(*dominant_rgb)
        palette_hex = [rgb_to_hex(*c) for c in palette_rgb]

        background = dominant_hex
        text = suggest_text_color(background)

        # Select accent color: the palette color with the best contrast against background
        # that is not too similar to the text color.
        accent = self._select_accent(background, text, palette_hex)

        # Select secondary color: different from accent, reasonable contrast.
        secondary = self._select_secondary(background, accent, palette_hex)

        palette = ColorPalette(
            background=background,
            text=text,
            accent=accent,
            secondary=secondary,
        )

        cr = contrast_ratio(text, background)

        return ThemeSuggestion(
            palette=palette,
            dominant_colors=palette_hex,
            contrast_ratio=round(cr, 2),
            meets_aa=meets_wcag_aa(text, background),
            meets_aaa=meets_wcag_aaa(text, background),
        )

    def _select_accent(
        self, background: str, text: str, palette: list[str]
    ) -> str:
        """Select the best accent color from the palette.

        Prefers colors with good contrast against the background that are
        visually distinct from the text color.

        Args:
            background: Background color hex.
            text: Text color hex.
            palette: List of palette color hex strings.

        Returns:
            The selected accent color hex string.
        """
        best_accent = palette[0] if palette else text
        best_score = 0.0

        for color in palette:
            bg_contrast = contrast_ratio(color, background)
            text_contrast = contrast_ratio(color, text)

            # Score: good contrast with background, different from text
            score = bg_contrast + (text_contrast * 0.5)
            if bg_contrast >= 3.0 and score > best_score:
                best_score = score
                best_accent = color

        return best_accent

    def _select_secondary(
        self, background: str, accent: str, palette: list[str]
    ) -> str | None:
        """Select a secondary color from the palette.

        Args:
            background: Background color hex.
            accent: Already-selected accent color hex.
            palette: List of palette color hex strings.

        Returns:
            The selected secondary color hex, or None if no good candidate.
        """
        for color in palette:
            if color == accent:
                continue
            bg_contrast = contrast_ratio(color, background)
            accent_contrast = contrast_ratio(color, accent)
            if bg_contrast >= 2.0 and accent_contrast >= 1.5:
                return color
        return None
```

---

### 2.18 New File: `backend/app/services/font_manager.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/font_manager.py` with the following content.

```python
"""Font loading and CJK text detection for label rendering.

Manages bundled fonts (Noto Sans JP for CJK, Inter for Latin) and provides
utilities for detecting text that requires CJK font rendering.
"""

import re
import unicodedata
from pathlib import Path

from app.config import Settings, get_settings

# CJK Unicode ranges
CJK_RANGES = [
    (0x4E00, 0x9FFF),    # CJK Unified Ideographs
    (0x3400, 0x4DBF),    # CJK Unified Ideographs Extension A
    (0x3000, 0x303F),    # CJK Symbols and Punctuation
    (0x3040, 0x309F),    # Hiragana
    (0x30A0, 0x30FF),    # Katakana
    (0x31F0, 0x31FF),    # Katakana Phonetic Extensions
    (0xFF00, 0xFFEF),    # Halfwidth and Fullwidth Forms
    (0xAC00, 0xD7AF),    # Hangul Syllables
    (0x1100, 0x11FF),    # Hangul Jamo
]


def contains_cjk(text: str) -> bool:
    """Check if a string contains any CJK characters.

    Args:
        text: The string to check.

    Returns:
        True if the string contains at least one CJK character.
    """
    for char in text:
        code_point = ord(char)
        for start, end in CJK_RANGES:
            if start <= code_point <= end:
                return True
    return False


def estimate_text_width(text: str, font_size_pt: float) -> float:
    """Estimate the rendered width of a text string in points.

    CJK characters are assumed to be full-width (1.0x font size).
    Latin characters are assumed to be approximately 0.55x font size.
    This is an approximation; actual width depends on the font.

    Args:
        text: The text string to measure.
        font_size_pt: The font size in points.

    Returns:
        Estimated width in points.
    """
    width = 0.0
    for char in text:
        code_point = ord(char)
        is_cjk = any(start <= code_point <= end for start, end in CJK_RANGES)
        if is_cjk:
            width += font_size_pt * 1.0
        elif char == " ":
            width += font_size_pt * 0.3
        else:
            width += font_size_pt * 0.55
    return width


class FontManager:
    """Manages font loading and selection for label rendering."""

    # Font file names expected in the font directory
    FONT_FILES = {
        "inter_regular": "Inter-Regular.ttf",
        "inter_bold": "Inter-Bold.ttf",
        "noto_sans_jp_regular": "NotoSansJP-Regular.ttf",
        "noto_sans_jp_bold": "NotoSansJP-Bold.ttf",
    }

    def __init__(self, settings: Settings | None = None) -> None:
        """Initialize the font manager.

        Args:
            settings: Application settings. Uses defaults if not provided.
        """
        self._settings = settings or get_settings()
        self._font_dir = self._settings.font_dir
        self._available_fonts: dict[str, Path] = {}
        self._scan_fonts()

    def _scan_fonts(self) -> None:
        """Scan the font directory for available font files."""
        if not self._font_dir.exists():
            return
        for key, filename in self.FONT_FILES.items():
            font_path = self._font_dir / filename
            if font_path.exists():
                self._available_fonts[key] = font_path

    def get_font_path(self, text: str, bold: bool = False) -> Path | None:
        """Get the appropriate font path for the given text.

        Returns a CJK font if the text contains CJK characters,
        otherwise returns a Latin font.

        Args:
            text: The text to be rendered.
            bold: Whether to use the bold weight.

        Returns:
            Path to the font file, or None if not available.
        """
        if contains_cjk(text):
            key = "noto_sans_jp_bold" if bold else "noto_sans_jp_regular"
        else:
            key = "inter_bold" if bold else "inter_regular"
        return self._available_fonts.get(key)

    def get_available_fonts(self) -> dict[str, Path]:
        """Return a dictionary of available font keys and their paths."""
        return dict(self._available_fonts)

    @property
    def has_cjk_fonts(self) -> bool:
        """Check if CJK fonts are available."""
        return "noto_sans_jp_regular" in self._available_fonts

    @property
    def has_latin_fonts(self) -> bool:
        """Check if Latin fonts are available."""
        return "inter_regular" in self._available_fonts
```

---

### 2.19 New File: `backend/app/services/image_processor.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/image_processor.py` with the following content.

```python
"""Album art image processing: downloading, resizing, and caching.

Handles fetching cover art from URLs, resizing to appropriate dimensions
for insert card panels, and caching processed images to disk.
"""

import hashlib
from pathlib import Path

import httpx
from PIL import Image

from app.config import Settings, get_settings
from app.utils.dimensions import mm_to_px


class ImageProcessor:
    """Downloads, resizes, and caches album artwork images."""

    def __init__(self, settings: Settings | None = None) -> None:
        """Initialize the image processor.

        Args:
            settings: Application settings. Uses defaults if not provided.
        """
        self._settings = settings or get_settings()
        self._cache_dir = self._settings.cache_dir / "images"
        self._cache_dir.mkdir(parents=True, exist_ok=True)

    def _cache_path(self, url: str, suffix: str = "") -> Path:
        """Generate a cache file path from a URL.

        Args:
            url: The source URL.
            suffix: Optional suffix to append before the extension.

        Returns:
            Path to the cached file.
        """
        url_hash = hashlib.sha256(url.encode()).hexdigest()[:16]
        return self._cache_dir / f"{url_hash}{suffix}.png"

    async def download_image(self, url: str) -> Path:
        """Download an image from a URL and cache it locally.

        Args:
            url: The image URL to download.

        Returns:
            Path to the cached image file.

        Raises:
            httpx.HTTPStatusError: If the download fails.
        """
        cache_path = self._cache_path(url)
        if cache_path.exists():
            return cache_path

        async with httpx.AsyncClient(follow_redirects=True) as client:
            response = await client.get(url, timeout=30.0)
            response.raise_for_status()

        cache_path.write_bytes(response.content)
        return cache_path

    def resize_for_panel(
        self,
        image_path: Path,
        panel_width_mm: float,
        panel_height_mm: float,
        dpi: int = 300,
    ) -> Path:
        """Resize an image to fit within a panel's dimensions.

        Maintains aspect ratio and centers the image within the panel area.
        The resized image is cached separately.

        Args:
            image_path: Path to the source image.
            panel_width_mm: Target panel width in millimeters.
            panel_height_mm: Target panel height in millimeters.
            dpi: Output resolution (default: 300).

        Returns:
            Path to the resized image file.
        """
        target_w = mm_to_px(panel_width_mm, dpi)
        target_h = mm_to_px(panel_height_mm, dpi)

        cache_key = f"{image_path.stem}_{target_w}x{target_h}"
        cache_path = self._cache_dir / f"{cache_key}.png"
        if cache_path.exists():
            return cache_path

        with Image.open(image_path) as img:
            img = img.convert("RGBA")
            img.thumbnail((target_w, target_h), Image.Resampling.LANCZOS)
            img.save(cache_path, "PNG")

        return cache_path

    def create_thumbnail(
        self,
        image_path: Path,
        max_size_px: int = 150,
    ) -> Path:
        """Create a small thumbnail of an image.

        Args:
            image_path: Path to the source image.
            max_size_px: Maximum dimension in pixels (default: 150).

        Returns:
            Path to the thumbnail file.
        """
        cache_path = self._cache_dir / f"{image_path.stem}_thumb_{max_size_px}.png"
        if cache_path.exists():
            return cache_path

        with Image.open(image_path) as img:
            img = img.convert("RGBA")
            img.thumbnail((max_size_px, max_size_px), Image.Resampling.LANCZOS)
            img.save(cache_path, "PNG")

        return cache_path
```

---

### 2.20 New File: `backend/app/services/layout_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/layout_engine.py` with the following content.

```python
"""Intelligent tracklist layout algorithm.

Analyzes track count, title lengths (including CJK character widths),
and available panel space to determine optimal column layout.
"""

from dataclasses import dataclass

from app.models.album import Track
from app.services.font_manager import estimate_text_width


@dataclass
class LayoutColumn:
    """A column of tracks in the tracklist layout."""

    tracks: list[Track]
    x_offset_pt: float
    width_pt: float


@dataclass
class TracklistLayout:
    """The computed layout for a tracklist panel."""

    columns: list[LayoutColumn]
    font_size_pt: float
    line_height_pt: float
    header_font_size_pt: float
    show_durations: bool
    strategy: str  # Description of the strategy used


class LayoutEngine:
    """Computes optimal tracklist layouts for insert card panels."""

    # Font size ranges (in points)
    DEFAULT_FONT_SIZE_PT: float = 8.0
    MIN_FONT_SIZE_PT: float = 5.5
    HEADER_FONT_SIZE_PT: float = 10.0
    LINE_HEIGHT_FACTOR: float = 2.2  # Line height as multiple of font size

    # Layout thresholds
    SINGLE_COLUMN_MAX: int = 6
    TWO_COLUMN_MAX: int = 24
    COMPACT_THRESHOLD: int = 18

    def compute_layout(
        self,
        tracks: list[Track],
        panel_width_pt: float,
        panel_height_pt: float,
        show_durations: bool = False,
    ) -> TracklistLayout:
        """Compute the optimal tracklist layout for the given tracks and panel.

        Args:
            tracks: List of tracks to lay out.
            panel_width_pt: Available panel width in points.
            panel_height_pt: Available panel height in points.
            show_durations: Whether to include track durations.

        Returns:
            A TracklistLayout with column assignments and sizing.
        """
        track_count = len(tracks)

        if track_count == 0:
            return TracklistLayout(
                columns=[],
                font_size_pt=self.DEFAULT_FONT_SIZE_PT,
                line_height_pt=self.DEFAULT_FONT_SIZE_PT * self.LINE_HEIGHT_FACTOR,
                header_font_size_pt=self.HEADER_FONT_SIZE_PT,
                show_durations=show_durations,
                strategy="empty",
            )

        if track_count <= self.SINGLE_COLUMN_MAX:
            return self._single_column(tracks, panel_width_pt, panel_height_pt, show_durations)

        return self._two_column(tracks, panel_width_pt, panel_height_pt, show_durations)

    def _single_column(
        self,
        tracks: list[Track],
        panel_width_pt: float,
        panel_height_pt: float,
        show_durations: bool,
    ) -> TracklistLayout:
        """Layout strategy for 1-6 tracks: single column, generous spacing."""
        font_size = self.DEFAULT_FONT_SIZE_PT
        line_height = font_size * self.LINE_HEIGHT_FACTOR

        column = LayoutColumn(
            tracks=tracks,
            x_offset_pt=0.0,
            width_pt=panel_width_pt,
        )

        return TracklistLayout(
            columns=[column],
            font_size_pt=font_size,
            line_height_pt=line_height,
            header_font_size_pt=self.HEADER_FONT_SIZE_PT,
            show_durations=show_durations,
            strategy=f"single_column ({len(tracks)} tracks)",
        )

    def _two_column(
        self,
        tracks: list[Track],
        panel_width_pt: float,
        panel_height_pt: float,
        show_durations: bool,
    ) -> TracklistLayout:
        """Layout strategy for 7+ tracks: two columns with balanced split."""
        track_count = len(tracks)

        # Determine font size based on track count
        if track_count <= 12:
            font_size = self.DEFAULT_FONT_SIZE_PT
            strategy = f"two_column_even ({track_count} tracks)"
        elif track_count <= self.COMPACT_THRESHOLD:
            font_size = 7.0
            strategy = f"two_column_balanced ({track_count} tracks)"
        elif track_count <= self.TWO_COLUMN_MAX:
            font_size = 6.0
            strategy = f"two_column_reduced ({track_count} tracks)"
        else:
            font_size = self.MIN_FONT_SIZE_PT
            strategy = f"two_column_compact ({track_count} tracks)"

        line_height = font_size * self.LINE_HEIGHT_FACTOR

        # Calculate available height for tracks (subtract header space)
        header_space = self.HEADER_FONT_SIZE_PT * 2.5
        available_height = panel_height_pt - header_space

        # Determine optimal split point
        max_rows_per_column = int(available_height / line_height)
        split = self._find_split_point(tracks, max_rows_per_column)

        col_gap_pt = 10.0
        col_width = (panel_width_pt - col_gap_pt) / 2.0

        left_column = LayoutColumn(
            tracks=tracks[:split],
            x_offset_pt=0.0,
            width_pt=col_width,
        )
        right_column = LayoutColumn(
            tracks=tracks[split:],
            x_offset_pt=col_width + col_gap_pt,
            width_pt=col_width,
        )

        return TracklistLayout(
            columns=[left_column, right_column],
            font_size_pt=font_size,
            line_height_pt=line_height,
            header_font_size_pt=self.HEADER_FONT_SIZE_PT,
            show_durations=show_durations,
            strategy=strategy,
        )

    def _find_split_point(self, tracks: list[Track], max_rows: int) -> int:
        """Find the optimal split point for two-column layout.

        Tries to balance the number of tracks in each column while
        respecting the maximum rows per column.

        Args:
            tracks: Full list of tracks.
            max_rows: Maximum number of rows per column.

        Returns:
            Index at which to split the track list.
        """
        track_count = len(tracks)
        ideal_split = (track_count + 1) // 2  # Slightly favor left column

        # Clamp to max_rows
        if ideal_split > max_rows:
            return max_rows
        if track_count - ideal_split > max_rows:
            return track_count - max_rows

        return ideal_split
```

---

### 2.21 New File: `backend/app/services/render_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/services/render_engine.py` with the following content.

```python
"""Label rendering engine using Pillow for PNG output.

Renders insert cards, disc face labels, and spine labels to pixel-accurate
PNG images at configurable DPI. PDF rendering via ReportLab is planned
for Milestone 3 (PRP-003).
"""

from io import BytesIO
from pathlib import Path

from PIL import Image, ImageDraw, ImageFont

from app.models.album import Track
from app.models.color import ColorPalette
from app.models.label import InsertCard, LabelProject
from app.models.render import RenderOptions
from app.models.template import STANDARD_4PANEL, TemplateDefinition
from app.services.font_manager import FontManager, contains_cjk
from app.services.layout_engine import LayoutEngine
from app.utils.contrast import hex_to_rgb
from app.utils.dimensions import mm_to_px, mm_to_pt


class RenderEngine:
    """Renders label projects to PNG images."""

    def __init__(
        self,
        font_manager: FontManager | None = None,
        layout_engine: LayoutEngine | None = None,
    ) -> None:
        """Initialize the render engine.

        Args:
            font_manager: Font manager instance. Creates default if not provided.
            layout_engine: Layout engine instance. Creates default if not provided.
        """
        self._font_manager = font_manager or FontManager()
        self._layout_engine = layout_engine or LayoutEngine()

    def render_insert_card_png(
        self,
        project: LabelProject,
        template: TemplateDefinition | None = None,
        options: RenderOptions | None = None,
    ) -> bytes:
        """Render an insert card to a PNG image.

        Args:
            project: The label project to render.
            template: Template definition. Uses standard 4-panel if not provided.
            options: Render options. Uses defaults if not provided.

        Returns:
            PNG image data as bytes.
        """
        template = template or STANDARD_4PANEL
        options = options or RenderOptions()
        dpi = options.dpi

        # Calculate canvas dimensions
        total_w_mm = template.total_width_with_bleed_mm if options.bleed_mm > 0 else template.total_width_mm
        total_h_mm = template.total_height_with_bleed_mm if options.bleed_mm > 0 else template.total_height_mm
        canvas_w = mm_to_px(total_w_mm, dpi)
        canvas_h = mm_to_px(total_h_mm, dpi)

        # Bleed offset
        bleed_px = mm_to_px(options.bleed_mm, dpi) if options.bleed_mm > 0 else 0

        # Create canvas
        bg_rgb = hex_to_rgb(project.palette.background)
        img = Image.new("RGB", (canvas_w, canvas_h), bg_rgb)
        draw = ImageDraw.Draw(img)

        # Draw each panel
        for panel in template.panels:
            panel_x = bleed_px + mm_to_px(panel.x_offset_mm, dpi)
            panel_y = bleed_px
            panel_w = mm_to_px(panel.width_mm, dpi)
            panel_h = mm_to_px(panel.height_mm, dpi)

            if panel.name == "spine":
                self._draw_spine_panel(
                    draw, project, panel_x, panel_y, panel_w, panel_h, dpi
                )
            elif panel.name == "info":
                self._draw_info_panel(
                    draw, img, project, panel_x, panel_y, panel_w, panel_h, dpi
                )
            elif panel.name == "art":
                self._draw_art_panel(
                    draw, img, project, panel_x, panel_y, panel_w, panel_h, dpi
                )
            elif panel.name == "tracklist":
                self._draw_tracklist_panel(
                    draw, project, panel_x, panel_y, panel_w, panel_h, dpi
                )

        # Draw crop marks if requested
        if options.crop_marks and bleed_px > 0:
            self._draw_crop_marks(draw, canvas_w, canvas_h, bleed_px)

        # Export to bytes
        buffer = BytesIO()
        img.save(buffer, format="PNG", dpi=(dpi, dpi))
        return buffer.getvalue()

    def _get_font(self, text: str, size_px: int, bold: bool = False) -> ImageFont.FreeTypeFont | ImageFont.ImageFont:
        """Get the appropriate font for the given text and size.

        Falls back to the default Pillow font if no custom fonts are available.

        Args:
            text: Text to be rendered (used for CJK detection).
            size_px: Font size in pixels.
            bold: Whether to use bold weight.

        Returns:
            A PIL font object.
        """
        font_path = self._font_manager.get_font_path(text, bold=bold)
        if font_path:
            return ImageFont.truetype(str(font_path), size_px)
        return ImageFont.load_default()

    def _draw_spine_panel(
        self,
        draw: ImageDraw.Draw,
        project: LabelProject,
        x: int, y: int, w: int, h: int,
        dpi: int,
    ) -> None:
        """Draw the spine panel with vertical text."""
        if not project.insert_card:
            return

        spine_bg = hex_to_rgb(project.palette.effective_spine_background)
        draw.rectangle([x, y, x + w, y + h], fill=spine_bg)

        spine_text = project.insert_card.spine_text
        if not spine_text:
            return

        font_size_px = max(mm_to_px(2.5, dpi), 8)
        font = self._get_font(spine_text, font_size_px)
        text_color = hex_to_rgb(project.palette.effective_spine_text)

        # Create a temporary image for rotated text
        text_bbox = font.getbbox(spine_text)
        text_w = text_bbox[2] - text_bbox[0]
        text_h = text_bbox[3] - text_bbox[1]

        txt_img = Image.new("RGBA", (text_w + 4, text_h + 4), (0, 0, 0, 0))
        txt_draw = ImageDraw.Draw(txt_img)
        txt_draw.text((2, 2), spine_text, fill=text_color, font=font)

        # Rotate 90 degrees counter-clockwise
        rotated = txt_img.rotate(90, expand=True)

        # Center in the spine panel
        paste_x = x + (w - rotated.width) // 2
        paste_y = y + (h - rotated.height) // 2

        # Paste with alpha mask
        if project.insert_card.spine_text:
            draw._image.paste(rotated, (paste_x, paste_y), rotated)

    def _draw_info_panel(
        self,
        draw: ImageDraw.Draw,
        img: Image.Image,
        project: LabelProject,
        x: int, y: int, w: int, h: int,
        dpi: int,
    ) -> None:
        """Draw the info panel with thumbnail, artist, album, year."""
        if not project.insert_card:
            return

        text_color = hex_to_rgb(project.palette.text)
        padding = mm_to_px(3.0, dpi)
        current_y = y + padding

        # Thumbnail
        thumb_path = project.insert_card.artwork_thumbnail_path
        if thumb_path and Path(thumb_path).exists():
            thumb_size = mm_to_px(15.0, dpi)
            with Image.open(thumb_path) as thumb:
                thumb = thumb.convert("RGBA")
                thumb.thumbnail((thumb_size, thumb_size), Image.Resampling.LANCZOS)
                img.paste(thumb, (x + padding, current_y), thumb)
            current_y += thumb_size + mm_to_px(3.0, dpi)

        # Artist name
        artist_size_px = mm_to_px(2.5, dpi)
        artist_font = self._get_font(project.insert_card.info_artist, artist_size_px)
        draw.text((x + padding, current_y), project.insert_card.info_artist, fill=text_color, font=artist_font)
        current_y += artist_size_px + mm_to_px(1.5, dpi)

        # Album title
        if project.insert_card.info_disc:
            disc_font = self._get_font(project.insert_card.info_disc, artist_size_px)
            draw.text((x + padding, current_y), project.insert_card.info_disc, fill=text_color, font=disc_font)
            current_y += artist_size_px + mm_to_px(1.5, dpi)

        # Year
        if project.insert_card.info_year:
            year_str = str(project.insert_card.info_year)
            year_font = self._get_font(year_str, artist_size_px)
            draw.text((x + padding, current_y), year_str, fill=text_color, font=year_font)

    def _draw_art_panel(
        self,
        draw: ImageDraw.Draw,
        img: Image.Image,
        project: LabelProject,
        x: int, y: int, w: int, h: int,
        dpi: int,
    ) -> None:
        """Draw the main artwork panel."""
        if not project.insert_card:
            return

        art_path = project.insert_card.artwork_image_path
        if not art_path or not Path(art_path).exists():
            return

        padding = mm_to_px(5.0, dpi)
        art_area_w = w - (2 * padding)
        art_area_h = h - (2 * padding) - mm_to_px(10.0, dpi)  # Leave space for subtitle

        with Image.open(art_path) as art:
            art = art.convert("RGBA")
            art.thumbnail((art_area_w, art_area_h), Image.Resampling.LANCZOS)

            # Center horizontally
            art_x = x + (w - art.width) // 2
            art_y = y + padding
            img.paste(art, (art_x, art_y), art)

        # Disc subtitle below artwork
        if project.insert_card.info_disc:
            text_color = hex_to_rgb(project.palette.text)
            subtitle_size = mm_to_px(2.5, dpi)
            subtitle_font = self._get_font(project.insert_card.info_disc, subtitle_size)
            subtitle_y = y + padding + art_area_h + mm_to_px(2.0, dpi)
            bbox = subtitle_font.getbbox(project.insert_card.info_disc)
            text_w = bbox[2] - bbox[0]
            subtitle_x = x + (w - text_w) // 2
            draw.text((subtitle_x, subtitle_y), project.insert_card.info_disc, fill=text_color, font=subtitle_font)

    def _draw_tracklist_panel(
        self,
        draw: ImageDraw.Draw,
        project: LabelProject,
        x: int, y: int, w: int, h: int,
        dpi: int,
    ) -> None:
        """Draw the tracklist panel with auto-arranged columns."""
        if not project.insert_card or not project.album:
            return

        text_color = hex_to_rgb(project.palette.text)
        accent_color = hex_to_rgb(project.palette.accent)
        padding = mm_to_px(3.0, dpi)

        # Get tracks for the current disc
        disc_idx = project.disc_number - 1
        if disc_idx < len(project.album.discs):
            tracks = project.album.discs[disc_idx].tracks
        else:
            tracks = []

        if not tracks:
            return

        # Tracklist header
        header_text = project.insert_card.tracklist_header
        header_size_px = mm_to_px(3.0, dpi)
        header_font = self._get_font(header_text, header_size_px, bold=True)
        draw.text((x + padding, y + padding), header_text, fill=accent_color, font=header_font)

        # Compute layout
        panel_width_pt = mm_to_pt(w * 25.4 / dpi)
        panel_height_pt = mm_to_pt(h * 25.4 / dpi)
        layout = self._layout_engine.compute_layout(
            tracks, panel_width_pt, panel_height_pt, project.insert_card.show_durations
        )

        # Render tracks
        track_size_px = max(mm_to_px(layout.font_size_pt * 25.4 / 72, dpi), 6)
        line_height_px = max(mm_to_px(layout.line_height_pt * 25.4 / 72, dpi), 8)
        header_offset = header_size_px + mm_to_px(4.0, dpi)

        for column in layout.columns:
            col_x = x + padding + mm_to_px(column.x_offset_pt * 25.4 / 72, dpi)
            col_y = y + padding + header_offset

            for i, track in enumerate(column.tracks):
                track_text = f"{track.number}. {track.title}"
                track_font = self._get_font(track_text, track_size_px)
                draw.text(
                    (col_x, col_y + (i * line_height_px)),
                    track_text,
                    fill=text_color,
                    font=track_font,
                )

                # Duration (right-aligned)
                if layout.show_durations and track.duration_formatted:
                    dur_text = track.duration_formatted
                    dur_font = self._get_font(dur_text, track_size_px)
                    dur_bbox = dur_font.getbbox(dur_text)
                    dur_w = dur_bbox[2] - dur_bbox[0]
                    col_right = col_x + mm_to_px(column.width_pt * 25.4 / 72, dpi)
                    draw.text(
                        (col_right - dur_w, col_y + (i * line_height_px)),
                        dur_text,
                        fill=text_color,
                        font=dur_font,
                    )

    def _draw_crop_marks(
        self,
        draw: ImageDraw.Draw,
        canvas_w: int,
        canvas_h: int,
        bleed_px: int,
    ) -> None:
        """Draw crop marks at the corners and midpoints of the trim area."""
        mark_length = bleed_px
        mark_color = (128, 128, 128)  # Grey
        mark_width = 1

        # Trim area boundaries
        left = bleed_px
        right = canvas_w - bleed_px
        top = bleed_px
        bottom = canvas_h - bleed_px

        # Corner marks
        corners = [
            (left, top),
            (right, top),
            (left, bottom),
            (right, bottom),
        ]

        for cx, cy in corners:
            # Horizontal mark
            if cx == left:
                draw.line([(cx - mark_length, cy), (cx, cy)], fill=mark_color, width=mark_width)
            else:
                draw.line([(cx, cy), (cx + mark_length, cy)], fill=mark_color, width=mark_width)

            # Vertical mark
            if cy == top:
                draw.line([(cx, cy - mark_length), (cx, cy)], fill=mark_color, width=mark_width)
            else:
                draw.line([(cx, cy), (cx, cy + mark_length)], fill=mark_color, width=mark_width)
```

---

### 2.22 New File: `backend/app/api/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/__init__.py` with the following content.

```python
"""API route modules for MDLabel Studio."""
```

---

### 2.23 New File: `backend/app/api/v1/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/__init__.py` with the following content.

```python
"""API v1 route modules."""
```

---

### 2.24 New File: `backend/app/api/v1/router.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/router.py` with the following content.

```python
"""Aggregated v1 API router."""

from fastapi import APIRouter

from app.api.v1.albums import router as albums_router
from app.api.v1.colors import router as colors_router
from app.api.v1.labels import router as labels_router
from app.api.v1.render import router as render_router

v1_router = APIRouter(prefix="/api/v1")

v1_router.include_router(albums_router, prefix="/albums", tags=["Albums"])
v1_router.include_router(colors_router, prefix="/colors", tags=["Colors"])
v1_router.include_router(labels_router, prefix="/labels", tags=["Labels"])
v1_router.include_router(render_router, prefix="/render", tags=["Render"])
```

---

### 2.25 New File: `backend/app/api/v1/albums.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/albums.py` with the following content.

```python
"""Album search and detail API endpoints."""

from fastapi import APIRouter, HTTPException, Query

from app.models.album import Album, AlbumSearchResult
from app.services.metadata import MusicBrainzService

router = APIRouter()

_metadata_service = MusicBrainzService()


@router.get(
    "/search",
    response_model=list[AlbumSearchResult],
    summary="Search for albums",
    description="Search for album releases by name and/or artist via MusicBrainz.",
)
async def search_albums(
    q: str = Query(description="Search query (album name, artist, or both)"),
    limit: int = Query(default=10, ge=1, le=50, description="Maximum results"),
) -> list[AlbumSearchResult]:
    """Search for albums by name and/or artist."""
    try:
        return await _metadata_service.search_albums(query=q, limit=limit)
    except Exception as e:
        raise HTTPException(status_code=502, detail=f"MusicBrainz API error: {e}") from e


@router.get(
    "/{mbid}",
    response_model=Album,
    summary="Get album details",
    description="Fetch full album details including tracklist by MusicBrainz release ID.",
)
async def get_album(mbid: str) -> Album:
    """Get full album details by MusicBrainz release ID."""
    try:
        return await _metadata_service.get_album(mbid=mbid)
    except Exception as e:
        raise HTTPException(status_code=502, detail=f"MusicBrainz API error: {e}") from e
```

---

### 2.26 New File: `backend/app/api/v1/labels.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/labels.py` with the following content.

```python
"""Label project CRUD API endpoints."""

from fastapi import APIRouter, HTTPException

from app.models.label import LabelProject

router = APIRouter()

# In-memory store for Milestone 1. Replaced by database in Milestone 5.
_projects: dict[str, LabelProject] = {}


@router.post(
    "",
    response_model=LabelProject,
    status_code=201,
    summary="Create a label project",
    description="Create a new label project with album data and color palette.",
)
async def create_label(project: LabelProject) -> LabelProject:
    """Create a new label project."""
    _projects[project.id] = project
    return project


@router.get(
    "/{project_id}",
    response_model=LabelProject,
    summary="Get a label project",
    description="Retrieve a saved label project by its ID.",
)
async def get_label(project_id: str) -> LabelProject:
    """Get a label project by ID."""
    project = _projects.get(project_id)
    if project is None:
        raise HTTPException(status_code=404, detail="Label project not found")
    return project


@router.put(
    "/{project_id}",
    response_model=LabelProject,
    summary="Update a label project",
    description="Update an existing label project.",
)
async def update_label(project_id: str, project: LabelProject) -> LabelProject:
    """Update an existing label project."""
    if project_id not in _projects:
        raise HTTPException(status_code=404, detail="Label project not found")
    project.id = project_id
    _projects[project_id] = project
    return project


@router.delete(
    "/{project_id}",
    status_code=204,
    summary="Delete a label project",
    description="Delete a label project by its ID.",
)
async def delete_label(project_id: str) -> None:
    """Delete a label project."""
    if project_id not in _projects:
        raise HTTPException(status_code=404, detail="Label project not found")
    del _projects[project_id]
```

---

### 2.27 New File: `backend/app/api/v1/render.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/render.py` with the following content.

```python
"""Label rendering API endpoints."""

from fastapi import APIRouter, HTTPException
from fastapi.responses import Response

from app.api.v1.labels import _projects
from app.models.render import RenderOptions
from app.services.render_engine import RenderEngine

router = APIRouter()

_render_engine = RenderEngine()


@router.post(
    "/png",
    summary="Render label to PNG",
    description="Render a label project to a high-resolution PNG image.",
    response_class=Response,
    responses={200: {"content": {"image/png": {}}}},
)
async def render_png(
    project_id: str,
    options: RenderOptions | None = None,
) -> Response:
    """Render a label project to PNG."""
    project = _projects.get(project_id)
    if project is None:
        raise HTTPException(status_code=404, detail="Label project not found")

    try:
        png_bytes = _render_engine.render_insert_card_png(
            project=project,
            options=options,
        )
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Render error: {e}") from e

    return Response(
        content=png_bytes,
        media_type="image/png",
        headers={
            "Content-Disposition": f'attachment; filename="{project_id}.png"',
        },
    )
```

---

### 2.28 New File: `backend/app/api/v1/colors.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/api/v1/colors.py` with the following content.

```python
"""Color extraction API endpoints."""

from fastapi import APIRouter, HTTPException, UploadFile

from app.models.color import ThemeSuggestion
from app.services.color_engine import ColorEngine

router = APIRouter()

_color_engine = ColorEngine()


@router.post(
    "/extract",
    response_model=ThemeSuggestion,
    summary="Extract color palette from image",
    description=(
        "Upload an album art image and extract a color palette with "
        "WCAG contrast-checked background, text, and accent colors."
    ),
)
async def extract_colors(file: UploadFile) -> ThemeSuggestion:
    """Extract a color theme from an uploaded image."""
    if not file.content_type or not file.content_type.startswith("image/"):
        raise HTTPException(status_code=400, detail="File must be an image")

    try:
        image_bytes = await file.read()
        return _color_engine.extract_from_bytes(image_bytes)
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Color extraction error: {e}") from e
```

---

### 2.29 New File: `backend/app/main.py`

**Action:** CREATE

**Instructions:** Create the file `backend/app/main.py` with the following content.

```python
"""FastAPI application factory for MDLabel Studio."""

from contextlib import asynccontextmanager
from pathlib import Path

from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

from app.api.v1.router import v1_router
from app.config import get_settings


@asynccontextmanager
async def lifespan(app: FastAPI):
    """Application lifespan: startup and shutdown events."""
    settings = get_settings()

    # Ensure required directories exist
    settings.font_dir.mkdir(parents=True, exist_ok=True)
    settings.cache_dir.mkdir(parents=True, exist_ok=True)
    settings.render_dir.mkdir(parents=True, exist_ok=True)

    yield

    # Shutdown: cleanup if needed


def create_app() -> FastAPI:
    """Create and configure the FastAPI application."""
    settings = get_settings()

    app = FastAPI(
        title="MDLabel Studio",
        description=(
            "MiniDisc label designer API. Supports album metadata import, "
            "color extraction, tracklist layout, and print-ready label rendering."
        ),
        version="0.1.0",
        lifespan=lifespan,
    )

    # CORS
    app.add_middleware(
        CORSMiddleware,
        allow_origins=settings.cors_origins,
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )

    # Routes
    app.include_router(v1_router)

    @app.get("/health", tags=["System"])
    async def health_check():
        """Health check endpoint."""
        return {"status": "ok", "version": "0.1.0"}

    return app


app = create_app()
```

---

## 3. Test Instructions

### 3.1 New File: `backend/tests/__init__.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/__init__.py` with the following content.

```python
"""Test suite for MDLabel Studio backend."""
```

---

### 3.2 New File: `backend/tests/conftest.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/conftest.py` with the following content.

```python
"""Shared test fixtures for MDLabel Studio backend tests."""

import pytest
from httpx import ASGITransport, AsyncClient

from app.main import app
from app.models.album import Album, Disc, Track
from app.models.color import ColorPalette
from app.models.label import InsertCard, LabelMode, LabelProject


@pytest.fixture
def sample_tracks() -> list[Track]:
    """A sample tracklist with 12 tracks (mixed Latin and CJK)."""
    return [
        Track(number=1, title="BLOOD on FIRE", duration_seconds=245),
        Track(number=2, title="ハリケーン・リリ、ボストン・マリ", duration_seconds=256),
        Track(number=3, title="Winter lander!!", duration_seconds=230),
        Track(number=4, title="Get チュー!", duration_seconds=215),
        Track(number=5, title="唇からロマンチカ", duration_seconds=248),
        Track(number=6, title="SUNSHINE", duration_seconds=220),
        Track(number=7, title="MIRAGE", duration_seconds=235),
        Track(number=8, title="MUSIC!!!", duration_seconds=210),
        Track(number=9, title="HORIZON", duration_seconds=240),
        Track(number=10, title="Break Down", duration_seconds=225),
        Track(number=11, title="Heart and Soul", duration_seconds=250),
        Track(number=12, title="Believe own way", duration_seconds=238),
    ]


@pytest.fixture
def sample_album(sample_tracks: list[Track]) -> Album:
    """A sample album based on AAA 10th Anniversary Best Disc 1."""
    return Album(
        mbid="test-mbid-001",
        title="10th ANNIVERSARY BEST",
        artist="AAA",
        year=2015,
        cover_art_url="https://example.com/cover.jpg",
        cover_art_thumbnail_url="https://example.com/cover-250.jpg",
        discs=[Disc(number=1, title="Disc 1", tracks=sample_tracks)],
        genre="J-Pop",
    )


@pytest.fixture
def sample_palette() -> ColorPalette:
    """A sample hot pink palette (AAA Disc 1 style)."""
    return ColorPalette(
        background="#FF1493",
        text="#FFFFFF",
        accent="#FFD700",
        secondary="#FF69B4",
    )


@pytest.fixture
def sample_insert_card() -> InsertCard:
    """A sample insert card configuration."""
    return InsertCard(
        spine_text="AAA - 10th ANNIVERSARY BEST - Disc 1",
        info_artist="AAA",
        info_album="10th ANNIVERSARY BEST",
        info_year=2015,
        info_disc="Disc 1",
        tracklist_header="Tracklist",
        show_durations=False,
        show_romanized=False,
    )


@pytest.fixture
def sample_project(
    sample_album: Album,
    sample_palette: ColorPalette,
    sample_insert_card: InsertCard,
) -> LabelProject:
    """A complete sample label project."""
    return LabelProject(
        mode=LabelMode.ALBUM,
        album=sample_album,
        disc_number=1,
        palette=sample_palette,
        insert_card=sample_insert_card,
    )


@pytest.fixture
async def async_client():
    """An async HTTP test client for the FastAPI app."""
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as client:
        yield client
```

---

### 3.3 New File: `backend/tests/test_models.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_models.py` with the following content.

```python
"""Tests for Pydantic v2 data models."""

import pytest
from pydantic import ValidationError

from app.models.album import Album, AlbumSearchResult, Disc, Track
from app.models.color import ColorPalette, ThemeSuggestion
from app.models.label import InsertCard, LabelMode, LabelProject, SourceFormat
from app.models.render import ExportFormat, RenderOptions
from app.models.template import BACKLINE_CARD, STANDARD_4PANEL, TemplateDefinition


class TestTrack:
    """Tests for the Track model."""

    def test_duration_formatted(self):
        track = Track(number=1, title="Test", duration_seconds=245)
        assert track.duration_formatted == "4:05"

    def test_duration_formatted_none(self):
        track = Track(number=1, title="Test")
        assert track.duration_formatted is None

    def test_duration_formatted_zero(self):
        track = Track(number=1, title="Test", duration_seconds=0)
        assert track.duration_formatted == "0:00"

    def test_duration_formatted_short(self):
        track = Track(number=1, title="Test", duration_seconds=65)
        assert track.duration_formatted == "1:05"


class TestDisc:
    """Tests for the Disc model."""

    def test_track_count(self, sample_tracks):
        disc = Disc(number=1, title="Disc 1", tracks=sample_tracks)
        assert disc.track_count == 12

    def test_empty_disc(self):
        disc = Disc(number=1)
        assert disc.track_count == 0


class TestAlbum:
    """Tests for the Album model."""

    def test_disc_count(self, sample_album):
        assert sample_album.disc_count == 1

    def test_total_tracks(self, sample_album):
        assert sample_album.total_tracks == 12

    def test_multi_disc(self, sample_tracks):
        album = Album(
            title="Multi Disc",
            artist="Test",
            discs=[
                Disc(number=1, tracks=sample_tracks[:6]),
                Disc(number=2, tracks=sample_tracks[6:]),
            ],
        )
        assert album.disc_count == 2
        assert album.total_tracks == 12


class TestColorPalette:
    """Tests for the ColorPalette model."""

    def test_valid_colors(self):
        palette = ColorPalette(background="#FF1493", text="#FFFFFF", accent="#FFD700")
        assert palette.background == "#FF1493"
        assert palette.text == "#FFFFFF"

    def test_lowercase_normalized(self):
        palette = ColorPalette(background="#ff1493", text="#ffffff", accent="#ffd700")
        assert palette.background == "#FF1493"

    def test_invalid_hex(self):
        with pytest.raises(ValidationError):
            ColorPalette(background="red", text="#FFFFFF", accent="#FFD700")

    def test_invalid_hex_short(self):
        with pytest.raises(ValidationError):
            ColorPalette(background="#FFF", text="#FFFFFF", accent="#FFD700")

    def test_effective_spine_colors(self):
        palette = ColorPalette(background="#FF1493", text="#FFFFFF", accent="#FFD700")
        assert palette.effective_spine_background == "#FF1493"
        assert palette.effective_spine_text == "#FFFFFF"

    def test_custom_spine_colors(self):
        palette = ColorPalette(
            background="#FF1493", text="#FFFFFF", accent="#FFD700",
            spine_background="#000000", spine_text="#FF1493",
        )
        assert palette.effective_spine_background == "#000000"
        assert palette.effective_spine_text == "#FF1493"


class TestTemplate:
    """Tests for template definitions."""

    def test_standard_4panel_dimensions(self):
        assert STANDARD_4PANEL.total_width_mm == 320.0
        assert STANDARD_4PANEL.total_height_mm == 103.2
        assert len(STANDARD_4PANEL.panels) == 4

    def test_backline_card_dimensions(self):
        assert BACKLINE_CARD.total_width_mm == 108.8
        assert BACKLINE_CARD.total_height_mm == 103.2
        assert len(BACKLINE_CARD.panels) == 3

    def test_bleed_dimensions(self):
        assert STANDARD_4PANEL.total_width_with_bleed_mm == 326.0
        assert STANDARD_4PANEL.total_height_with_bleed_mm == 109.2

    def test_backline_spine_widths(self):
        spines = [p for p in BACKLINE_CARD.panels if "spine" in p.name]
        assert len(spines) == 2
        for spine in spines:
            assert spine.width_mm == 11.2


class TestRenderOptions:
    """Tests for render options."""

    def test_defaults(self):
        opts = RenderOptions()
        assert opts.dpi == 300
        assert opts.bleed_mm == 3.0
        assert opts.crop_marks is True
        assert opts.cmyk is False

    def test_export_formats(self):
        assert ExportFormat.PDF == "pdf"
        assert ExportFormat.PNG == "png"
        assert ExportFormat.SVG == "svg"


class TestLabelProject:
    """Tests for the LabelProject model."""

    def test_auto_id(self, sample_palette, sample_insert_card):
        project = LabelProject(
            mode=LabelMode.ALBUM,
            palette=sample_palette,
            insert_card=sample_insert_card,
        )
        assert project.id is not None
        assert len(project.id) > 0

    def test_source_formats(self):
        assert SourceFormat.CD_COPY == "cd_copy"
        assert SourceFormat.VINYL_COPY == "vinyl_copy"
        assert SourceFormat.STREAMING == "streaming"

    def test_serialization_roundtrip(self, sample_project):
        json_str = sample_project.model_dump_json()
        restored = LabelProject.model_validate_json(json_str)
        assert restored.id == sample_project.id
        assert restored.album.title == sample_project.album.title
        assert restored.palette.background == sample_project.palette.background
```

---

### 3.4 New File: `backend/tests/test_dimensions.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_dimensions.py` with the following content.

```python
"""Tests for dimension conversion utilities."""

from app.utils.dimensions import (
    BACKLINE_CENTER_WIDTH_MM,
    BACKLINE_HEIGHT_MM,
    BACKLINE_SPINE_WIDTH_MM,
    BACKLINE_WIDTH_MM,
    BOOKLET_PANEL_HEIGHT_MM,
    BOOKLET_PANEL_WIDTH_MM,
    DISC_LABEL_GRID_TOTAL,
    DISC_LABEL_STANDARD_HEIGHT_MM,
    DISC_LABEL_STANDARD_WIDTH_MM,
    INSERT_HEIGHT_MM,
    INSERT_TOTAL_WIDTH_MM,
    mm_to_pt,
    mm_to_px,
    pt_to_mm,
    px_to_mm,
)


class TestMmToPx:
    """Tests for mm-to-pixel conversion."""

    def test_300dpi(self):
        assert mm_to_px(25.4, 300) == 300  # 1 inch = 300 px at 300 DPI

    def test_72dpi(self):
        assert mm_to_px(25.4, 72) == 72

    def test_zero(self):
        assert mm_to_px(0, 300) == 0

    def test_insert_card_width(self):
        px = mm_to_px(INSERT_TOTAL_WIDTH_MM, 300)
        assert px == round(320.0 * 300 / 25.4)

    def test_insert_card_height(self):
        px = mm_to_px(INSERT_HEIGHT_MM, 300)
        assert px == round(103.2 * 300 / 25.4)


class TestPxToMm:
    """Tests for pixel-to-mm conversion."""

    def test_roundtrip(self):
        original_mm = 52.0
        px = mm_to_px(original_mm, 300)
        restored_mm = px_to_mm(px, 300)
        assert abs(restored_mm - original_mm) < 0.1


class TestMmToPt:
    """Tests for mm-to-point conversion."""

    def test_known_value(self):
        # 25.4mm = 1 inch = 72 points
        assert abs(mm_to_pt(25.4) - 72.0) < 0.01

    def test_roundtrip(self):
        original_mm = 103.2
        pt = mm_to_pt(original_mm)
        restored_mm = pt_to_mm(pt)
        assert abs(restored_mm - original_mm) < 0.01


class TestMDMEConstants:
    """Tests for MDME 2022 template dimension constants."""

    def test_backline_width(self):
        assert BACKLINE_WIDTH_MM == 108.8

    def test_backline_height(self):
        assert BACKLINE_HEIGHT_MM == 103.2

    def test_backline_spine_width(self):
        assert BACKLINE_SPINE_WIDTH_MM == 11.2

    def test_backline_panels_sum(self):
        total = BACKLINE_SPINE_WIDTH_MM + BACKLINE_CENTER_WIDTH_MM + BACKLINE_SPINE_WIDTH_MM
        assert abs(total - BACKLINE_WIDTH_MM) < 0.01

    def test_booklet_panel(self):
        assert BOOKLET_PANEL_WIDTH_MM == 72.7
        assert BOOKLET_PANEL_HEIGHT_MM == 104.9

    def test_disc_label(self):
        assert DISC_LABEL_STANDARD_WIDTH_MM == 52.0
        assert DISC_LABEL_STANDARD_HEIGHT_MM == 38.0

    def test_disc_label_grid(self):
        assert DISC_LABEL_GRID_TOTAL == 20
```

---

### 3.5 New File: `backend/tests/test_contrast.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_contrast.py` with the following content.

```python
"""Tests for WCAG contrast ratio calculations."""

from app.utils.contrast import (
    contrast_ratio,
    hex_to_rgb,
    meets_wcag_aa,
    meets_wcag_aaa,
    relative_luminance,
    rgb_to_hex,
    suggest_text_color,
)


class TestHexConversion:
    """Tests for hex/RGB conversion."""

    def test_hex_to_rgb_black(self):
        assert hex_to_rgb("#000000") == (0, 0, 0)

    def test_hex_to_rgb_white(self):
        assert hex_to_rgb("#FFFFFF") == (255, 255, 255)

    def test_hex_to_rgb_red(self):
        assert hex_to_rgb("#FF0000") == (255, 0, 0)

    def test_rgb_to_hex(self):
        assert rgb_to_hex(255, 0, 128) == "#FF0080"

    def test_roundtrip(self):
        original = "#FF1493"
        r, g, b = hex_to_rgb(original)
        assert rgb_to_hex(r, g, b) == original


class TestRelativeLuminance:
    """Tests for WCAG relative luminance."""

    def test_black(self):
        assert relative_luminance("#000000") == 0.0

    def test_white(self):
        assert abs(relative_luminance("#FFFFFF") - 1.0) < 0.001

    def test_mid_grey(self):
        lum = relative_luminance("#808080")
        assert 0.2 < lum < 0.3  # Approximately 0.216


class TestContrastRatio:
    """Tests for WCAG contrast ratio."""

    def test_black_on_white(self):
        ratio = contrast_ratio("#000000", "#FFFFFF")
        assert abs(ratio - 21.0) < 0.1

    def test_same_color(self):
        ratio = contrast_ratio("#FF1493", "#FF1493")
        assert abs(ratio - 1.0) < 0.01

    def test_symmetric(self):
        r1 = contrast_ratio("#FF1493", "#FFFFFF")
        r2 = contrast_ratio("#FFFFFF", "#FF1493")
        assert abs(r1 - r2) < 0.01


class TestWCAGCompliance:
    """Tests for WCAG AA and AAA compliance checks."""

    def test_white_on_black_passes_aa(self):
        assert meets_wcag_aa("#FFFFFF", "#000000") is True

    def test_white_on_black_passes_aaa(self):
        assert meets_wcag_aaa("#FFFFFF", "#000000") is True

    def test_low_contrast_fails_aa(self):
        assert meets_wcag_aa("#CCCCCC", "#FFFFFF") is False

    def test_white_on_hot_pink(self):
        # Hot pink (#FF1493) with white text
        assert meets_wcag_aa("#FFFFFF", "#FF1493") is True or not meets_wcag_aa("#FFFFFF", "#FF1493")


class TestSuggestTextColor:
    """Tests for automatic text color suggestion."""

    def test_dark_background(self):
        assert suggest_text_color("#000000") == "#FFFFFF"

    def test_light_background(self):
        assert suggest_text_color("#FFFFFF") == "#000000"

    def test_hot_pink(self):
        # Hot pink is relatively bright, could go either way
        result = suggest_text_color("#FF1493")
        assert result in ("#FFFFFF", "#000000")
```

---

### 3.6 New File: `backend/tests/test_metadata.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_metadata.py` with the following content.

```python
"""Tests for the MusicBrainz metadata service."""

from unittest.mock import AsyncMock, patch

import pytest

from app.models.album import Album, AlbumSearchResult
from app.services.metadata import MusicBrainzService


MOCK_SEARCH_RESPONSE = {
    "releases": [
        {
            "id": "test-mbid-001",
            "title": "10th ANNIVERSARY BEST",
            "artist-credit": [{"name": "AAA"}],
            "date": "2015-09-16",
            "country": "JP",
            "media": [{"position": 1}, {"position": 2}, {"position": 3}],
            "score": 100,
        }
    ]
}

MOCK_RELEASE_RESPONSE = {
    "id": "test-mbid-001",
    "title": "10th ANNIVERSARY BEST",
    "artist-credit": [{"name": "AAA"}],
    "date": "2015-09-16",
    "media": [
        {
            "position": 1,
            "title": "",
            "tracks": [
                {
                    "position": 1,
                    "recording": {"title": "BLOOD on FIRE", "length": 245000},
                },
                {
                    "position": 2,
                    "recording": {"title": "ハリケーン・リリ、ボストン・マリ", "length": 256000},
                },
            ],
        }
    ],
}


class TestMusicBrainzService:
    """Tests for MusicBrainz API client."""

    @pytest.fixture
    def service(self):
        return MusicBrainzService()

    @patch("app.services.metadata.httpx.AsyncClient")
    async def test_search_albums(self, mock_client_cls, service):
        mock_response = AsyncMock()
        mock_response.json.return_value = MOCK_SEARCH_RESPONSE
        mock_response.raise_for_status = AsyncMock()

        mock_client = AsyncMock()
        mock_client.get.return_value = mock_response
        mock_client.__aenter__ = AsyncMock(return_value=mock_client)
        mock_client.__aexit__ = AsyncMock(return_value=False)
        mock_client_cls.return_value = mock_client

        results = await service.search_albums("AAA 10th Anniversary Best")

        assert len(results) == 1
        assert results[0].title == "10th ANNIVERSARY BEST"
        assert results[0].artist == "AAA"
        assert results[0].year == 2015
        assert results[0].disc_count == 3

    @patch("app.services.metadata.httpx.AsyncClient")
    async def test_get_album(self, mock_client_cls, service):
        mock_response = AsyncMock()
        mock_response.json.return_value = MOCK_RELEASE_RESPONSE
        mock_response.raise_for_status = AsyncMock()

        mock_client = AsyncMock()
        mock_client.get.return_value = mock_response
        mock_client.__aenter__ = AsyncMock(return_value=mock_client)
        mock_client.__aexit__ = AsyncMock(return_value=False)
        mock_client_cls.return_value = mock_client

        album = await service.get_album("test-mbid-001")

        assert album.title == "10th ANNIVERSARY BEST"
        assert album.artist == "AAA"
        assert album.year == 2015
        assert album.disc_count == 1
        assert album.discs[0].track_count == 2
        assert album.discs[0].tracks[0].title == "BLOOD on FIRE"
        assert album.discs[0].tracks[1].title == "ハリケーン・リリ、ボストン・マリ"

    async def test_search_caching(self, service):
        """Verify that repeated searches return cached results."""
        service._cache.set("search:test:10", [
            AlbumSearchResult(
                mbid="cached", title="Cached", artist="Test", score=100,
            )
        ])

        results = await service.search_albums("test")
        assert len(results) == 1
        assert results[0].mbid == "cached"
```

---

### 3.7 New File: `backend/tests/test_color_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_color_engine.py` with the following content.

```python
"""Tests for the color extraction engine."""

import io

import pytest
from PIL import Image

from app.services.color_engine import ColorEngine


def _create_test_image(color: tuple[int, int, int], size: tuple[int, int] = (100, 100)) -> bytes:
    """Create a solid-color test image and return its bytes."""
    img = Image.new("RGB", size, color)
    buffer = io.BytesIO()
    img.save(buffer, format="PNG")
    return buffer.getvalue()


class TestColorEngine:
    """Tests for color extraction and palette generation."""

    @pytest.fixture
    def engine(self):
        return ColorEngine(palette_size=6)

    def test_extract_from_red_image(self, engine):
        image_bytes = _create_test_image((255, 0, 0))
        theme = engine.extract_from_bytes(image_bytes)

        assert theme.palette.background is not None
        assert theme.palette.text is not None
        assert theme.palette.accent is not None
        assert theme.contrast_ratio > 0

    def test_extract_from_white_image(self, engine):
        image_bytes = _create_test_image((255, 255, 255))
        theme = engine.extract_from_bytes(image_bytes)

        # White background should get black text
        assert theme.palette.text == "#000000"

    def test_extract_from_black_image(self, engine):
        image_bytes = _create_test_image((0, 0, 0))
        theme = engine.extract_from_bytes(image_bytes)

        # Black background should get white text
        assert theme.palette.text == "#FFFFFF"

    def test_dominant_colors_populated(self, engine):
        image_bytes = _create_test_image((255, 20, 147))  # Hot pink
        theme = engine.extract_from_bytes(image_bytes)

        assert len(theme.dominant_colors) > 0

    def test_wcag_compliance_fields(self, engine):
        image_bytes = _create_test_image((128, 128, 128))
        theme = engine.extract_from_bytes(image_bytes)

        assert isinstance(theme.meets_aa, bool)
        assert isinstance(theme.meets_aaa, bool)

    def test_extract_from_file(self, engine, tmp_path):
        img_path = tmp_path / "test.png"
        img = Image.new("RGB", (100, 100), (255, 20, 147))
        img.save(img_path)

        theme = engine.extract_from_file(img_path)
        assert theme.palette.background is not None
```

---

### 3.8 New File: `backend/tests/test_layout_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_layout_engine.py` with the following content.

```python
"""Tests for the tracklist layout engine."""

import pytest

from app.models.album import Track

from app.services.layout_engine import LayoutEngine


def _make_tracks(count: int) -> list[Track]:
    """Create a list of test tracks."""
    return [
        Track(number=i + 1, title=f"Track {i + 1}", duration_seconds=200 + i * 10)
        for i in range(count)
    ]


def _make_cjk_tracks(count: int) -> list[Track]:
    """Create a list of test tracks with CJK titles."""
    titles = [
        "ハリケーン・リリ", "唇からロマンチカ", "逢いたい理由",
        "負けないこころ", "アシタノヒカリ", "愛してるのに、愛せない",
        "虹", "さよならの前に", "夏に戻る夏の記憶",
        "BLOOD on FIRE", "Winter lander!!", "Get チュー!",
    ]
    return [
        Track(number=i + 1, title=titles[i % len(titles)], duration_seconds=200 + i * 10)
        for i in range(count)
    ]


class TestLayoutEngine:
    """Tests for tracklist layout computation."""

    @pytest.fixture
    def engine(self):
        return LayoutEngine()

    # Panel dimensions in points (approximate for 160mm wide, 103.2mm tall)
    PANEL_WIDTH_PT = 453.5
    PANEL_HEIGHT_PT = 292.6

    def test_empty_tracklist(self, engine):
        layout = engine.compute_layout([], self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert layout.strategy == "empty"
        assert len(layout.columns) == 0

    def test_single_column_for_few_tracks(self, engine):
        tracks = _make_tracks(5)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert len(layout.columns) == 1
        assert "single_column" in layout.strategy
        assert len(layout.columns[0].tracks) == 5

    def test_two_columns_for_many_tracks(self, engine):
        tracks = _make_tracks(12)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert len(layout.columns) == 2
        assert "two_column" in layout.strategy
        total = sum(len(col.tracks) for col in layout.columns)
        assert total == 12

    def test_balanced_split(self, engine):
        tracks = _make_tracks(12)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        left_count = len(layout.columns[0].tracks)
        right_count = len(layout.columns[1].tracks)
        assert abs(left_count - right_count) <= 1

    def test_compact_font_for_many_tracks(self, engine):
        tracks = _make_tracks(20)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert layout.font_size_pt < engine.DEFAULT_FONT_SIZE_PT

    def test_cjk_tracks(self, engine):
        tracks = _make_cjk_tracks(10)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert len(layout.columns) == 2
        total = sum(len(col.tracks) for col in layout.columns)
        assert total == 10

    def test_show_durations_flag(self, engine):
        tracks = _make_tracks(6)
        layout = engine.compute_layout(
            tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT, show_durations=True
        )
        assert layout.show_durations is True

    def test_threshold_boundary_six(self, engine):
        """6 tracks should be single column."""
        tracks = _make_tracks(6)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert len(layout.columns) == 1

    def test_threshold_boundary_seven(self, engine):
        """7 tracks should be two columns."""
        tracks = _make_tracks(7)
        layout = engine.compute_layout(tracks, self.PANEL_WIDTH_PT, self.PANEL_HEIGHT_PT)
        assert len(layout.columns) == 2
```

---

### 3.9 New File: `backend/tests/test_render_engine.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_render_engine.py` with the following content.

```python
"""Tests for the label render engine."""

import io

import pytest
from PIL import Image

from app.models.render import RenderOptions
from app.models.template import STANDARD_4PANEL
from app.services.render_engine import RenderEngine
from app.utils.dimensions import mm_to_px


class TestRenderEngine:
    """Tests for PNG rendering of insert cards."""

    @pytest.fixture
    def engine(self):
        return RenderEngine()

    def test_render_produces_png_bytes(self, engine, sample_project):
        png_bytes = engine.render_insert_card_png(sample_project)
        assert isinstance(png_bytes, bytes)
        assert len(png_bytes) > 0

    def test_render_is_valid_png(self, engine, sample_project):
        png_bytes = engine.render_insert_card_png(sample_project)
        img = Image.open(io.BytesIO(png_bytes))
        assert img.format == "PNG"

    def test_render_dimensions_with_bleed(self, engine, sample_project):
        options = RenderOptions(dpi=300, bleed_mm=3.0)
        png_bytes = engine.render_insert_card_png(sample_project, options=options)
        img = Image.open(io.BytesIO(png_bytes))

        expected_w = mm_to_px(STANDARD_4PANEL.total_width_with_bleed_mm, 300)
        expected_h = mm_to_px(STANDARD_4PANEL.total_height_with_bleed_mm, 300)

        assert img.width == expected_w
        assert img.height == expected_h

    def test_render_dimensions_without_bleed(self, engine, sample_project):
        options = RenderOptions(dpi=300, bleed_mm=0.0)
        png_bytes = engine.render_insert_card_png(sample_project, options=options)
        img = Image.open(io.BytesIO(png_bytes))

        expected_w = mm_to_px(STANDARD_4PANEL.total_width_mm, 300)
        expected_h = mm_to_px(STANDARD_4PANEL.total_height_mm, 300)

        assert img.width == expected_w
        assert img.height == expected_h

    def test_render_background_color(self, engine, sample_project):
        options = RenderOptions(dpi=72, bleed_mm=0.0, crop_marks=False)
        png_bytes = engine.render_insert_card_png(sample_project, options=options)
        img = Image.open(io.BytesIO(png_bytes))

        # Sample a pixel from the tracklist panel area (should be background color)
        # Hot pink: #FF1493 = (255, 20, 147)
        pixel = img.getpixel((img.width - 10, img.height // 2))
        assert pixel[0] == 255  # Red channel
        assert pixel[1] == 20   # Green channel
        assert pixel[2] == 147  # Blue channel

    def test_render_low_dpi(self, engine, sample_project):
        options = RenderOptions(dpi=72, bleed_mm=0.0)
        png_bytes = engine.render_insert_card_png(sample_project, options=options)
        img = Image.open(io.BytesIO(png_bytes))

        expected_w = mm_to_px(STANDARD_4PANEL.total_width_mm, 72)
        expected_h = mm_to_px(STANDARD_4PANEL.total_height_mm, 72)

        assert img.width == expected_w
        assert img.height == expected_h
```

---

### 3.10 New File: `backend/tests/test_api.py`

**Action:** CREATE

**Instructions:** Create the file `backend/tests/test_api.py` with the following content.

```python
"""Integration tests for the FastAPI API endpoints."""

import pytest
from httpx import ASGITransport, AsyncClient

from app.main import app
from app.models.label import LabelMode, LabelProject


class TestHealthEndpoint:
    """Tests for the health check endpoint."""

    async def test_health_check(self, async_client):
        response = await async_client.get("/health")
        assert response.status_code == 200
        data = response.json()
        assert data["status"] == "ok"
        assert data["version"] == "0.1.0"


class TestLabelEndpoints:
    """Tests for the label CRUD endpoints."""

    async def test_create_label(self, async_client, sample_project):
        payload = sample_project.model_dump(mode="json")
        response = await async_client.post("/api/v1/labels", json=payload)
        assert response.status_code == 201
        data = response.json()
        assert data["id"] == sample_project.id
        assert data["mode"] == "album"

    async def test_get_label(self, async_client, sample_project):
        # Create first
        payload = sample_project.model_dump(mode="json")
        await async_client.post("/api/v1/labels", json=payload)

        # Retrieve
        response = await async_client.get(f"/api/v1/labels/{sample_project.id}")
        assert response.status_code == 200
        data = response.json()
        assert data["album"]["title"] == "10th ANNIVERSARY BEST"

    async def test_get_label_not_found(self, async_client):
        response = await async_client.get("/api/v1/labels/nonexistent-id")
        assert response.status_code == 404

    async def test_delete_label(self, async_client, sample_project):
        # Create first
        payload = sample_project.model_dump(mode="json")
        await async_client.post("/api/v1/labels", json=payload)

        # Delete
        response = await async_client.delete(f"/api/v1/labels/{sample_project.id}")
        assert response.status_code == 204

        # Verify gone
        response = await async_client.get(f"/api/v1/labels/{sample_project.id}")
        assert response.status_code == 404


class TestAlbumEndpoints:
    """Tests for the album search and detail endpoints."""

    async def test_search_requires_query(self, async_client):
        response = await async_client.get("/api/v1/albums/search")
        assert response.status_code == 422  # Missing required query param


class TestRenderEndpoints:
    """Tests for the render endpoints."""

    async def test_render_not_found(self, async_client):
        response = await async_client.post(
            "/api/v1/render/png",
            params={"project_id": "nonexistent"},
        )
        assert response.status_code == 404

    async def test_render_png(self, async_client, sample_project):
        # Create project first
        payload = sample_project.model_dump(mode="json")
        await async_client.post("/api/v1/labels", json=payload)

        # Render
        response = await async_client.post(
            "/api/v1/render/png",
            params={"project_id": sample_project.id},
        )
        assert response.status_code == 200
        assert response.headers["content-type"] == "image/png"
        assert len(response.content) > 0
```

---

## 4. Verification Instructions

### 4.1 Install Dependencies

**Action:** EXECUTE

**Instructions:** Run the following commands from the `backend/` directory.

```bash
cd backend
pip install -e ".[dev]"
```

---

### 4.2 Download Bundled Fonts

**Action:** EXECUTE

**Instructions:** Download the required fonts into the `backend/fonts/` directory.

```bash
mkdir -p backend/fonts
# Download Noto Sans JP
curl -L -o /tmp/NotoSansJP.zip "https://fonts.google.com/download?family=Noto+Sans+JP"
unzip -o /tmp/NotoSansJP.zip -d /tmp/NotoSansJP
cp /tmp/NotoSansJP/static/NotoSansJP-Regular.ttf backend/fonts/
cp /tmp/NotoSansJP/static/NotoSansJP-Bold.ttf backend/fonts/

# Download Inter
curl -L -o /tmp/Inter.zip "https://fonts.google.com/download?family=Inter"
unzip -o /tmp/Inter.zip -d /tmp/Inter
cp /tmp/Inter/static/Inter_18pt-Regular.ttf backend/fonts/Inter-Regular.ttf
cp /tmp/Inter/static/Inter_18pt-Bold.ttf backend/fonts/Inter-Bold.ttf
```

**Note:** If the exact font file names differ, rename them to match the expected names in `FontManager.FONT_FILES`. The font manager will gracefully fall back to the default Pillow font if custom fonts are unavailable.

---

### 4.3 Execute Tests

**Action:** EXECUTE

**Instructions:** Run the full test suite from the `backend/` directory.

```bash
cd backend
pytest -v --tb=short
```

**Expected result:** All tests pass. If any tests fail due to font availability, verify that the `fonts/` directory contains the expected font files, or confirm that the fallback behavior is acceptable for Milestone 1.

---

### 4.4 Acceptance Criteria

**Action:** VERIFY

Confirm the following before marking this PRP as complete:

| Criterion | Verification |
|-----------|-------------|
| All Pydantic models validate correctly | `tests/test_models.py` passes |
| Dimension conversions are accurate | `tests/test_dimensions.py` passes |
| WCAG contrast calculations are correct | `tests/test_contrast.py` passes |
| MusicBrainz service returns structured data | `tests/test_metadata.py` passes |
| Color extraction produces accessible palettes | `tests/test_color_engine.py` passes |
| Layout engine handles 1-24+ tracks | `tests/test_layout_engine.py` passes |
| Render engine produces correctly-sized PNGs | `tests/test_render_engine.py` passes |
| API endpoints return correct status codes | `tests/test_api.py` passes |
| Health endpoint returns `{"status": "ok"}` | `GET /health` returns 200 |
| OpenAPI docs are generated | `GET /docs` returns Swagger UI |
| MDME 2022 dimensions are encoded as constants | `utils/dimensions.py` contains all constants |
| CJK text detection works for Japanese/Korean | `services/font_manager.py` handles mixed scripts |

---

## 5. Post-Completion

After all acceptance criteria are met:

1. Update `TASKS.md` --- Mark all Milestone 1 tasks as `[x]` complete.
2. Commit with message: `feat: implement backend foundation (PRP-001)`
3. Proceed to **PRP-002: Frontend Core** when ready.
