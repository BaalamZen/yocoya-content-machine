# YOCOYA CONTENT MACHINE CHANGELOG v2.0
**Date:** February 22, 2026
**Phase:** Asset Pipeline Architecture & Production Readiness
**Status:** COMPLETE - Two-tier asset system defined, 20 assets cataloged, documentation updated

---

## SESSION SUMMARY

### COMPLETED
- **Freepik MCP Server** - 9-tool server built and tested for automated asset search, download, and attribution tracking
- **10 Manual Asset Downloads** - Checkmarks (3), speech bubbles (6), firewall icon (1) downloaded from Freepik browser
- **Attribution Tracker** - 20 entries in `asset-attribution.json` (10 MCP-automated + 10 manually cataloged)
- **Two-Tier Asset System** - Architecture decision: EPS sheets = reference catalog, individual PNGs = production-ready
- **Video Tool Compatibility Audit** - Confirmed no affiliate video tool can use EPS programmatically
- **Style Guide Update** - Added production pipeline section with file format strategy and workflow asset lookup
- **Memory & Documentation** - Updated `content-machine.md`, `STYLE-GUIDE.md`, and this changelog

---

## FREEPIK MCP SERVER

### Server Details
- **Location:** `yocoya.ai/mcp-servers/freepik/server.py`
- **Pattern:** FastMCP + httpx (same as webflow-data server)
- **Config:** `.mcp.json` with `FREEPIK_API_KEY` env var, runs via `uv run`
- **API Key:** Essential plan — cannot download Premium assets via API (403 error)

### 9 Tools
| Tool | Purpose |
|---|---|
| `search_resources` | Search vectors, photos, PSDs (filters: type, license, color, orientation) |
| `get_resource_detail` | Full metadata: author, license, formats, dimensions, tags |
| `get_download_url` | Get download link (default format) |
| `get_download_by_format` | Get download link for specific format (eps, svg, png, psd, ai) |
| `search_icons` | Search Flaticon icons (shape, color, type filters) |
| `download_and_track` | Download file + auto-log attribution |
| `list_tracked_assets` | List tracked downloads (filter by category/subfolder) |
| `attribution_summary` | Summary by category, license, AI-generated status |

---

## ASSET DOWNLOADS (10 new, 20 total)

### Manual Download Process
10 zip files downloaded from Freepik browser → extracted → renamed per naming convention → copied to yocoya-fx subfolders → attribution tracker updated manually.

### Resource ID Lookup
- 7 of 10 resource IDs found via Freepik search API
- 3 could not be identified (firewall icon, blue balloon, green balloon) — used descriptive slugs instead
- rawpixel.com balloon series: purple found at ID 30086345, but blue/green not at predictable adjacent IDs

### New Assets

**Checkmarks (3)**
| File | Author | Style | Resource ID |
|---|---|---|---|
| `checkmarks-001-18141290.eps` | starline | Glossy 3D check/cross buttons | 18141290 |
| `checkmarks-002-57456907.psd` | team toucan | 3D green checkmark (Premium PSD) | 57456907 |
| `checkmarks-003-24058405.eps` | WangXiNa | Wireframe/connecting dots | 24058405 |

**AI Icons (1)**
| File | Author | Style | Resource ID |
|---|---|---|---|
| `ai-icons-002-firewall.eps` | unknown | Firewall/security concept | unknown |

**Speech Bubbles (6)**
| File | Author | Style | Resource ID |
|---|---|---|---|
| `speech-bubbles-001-5551727.eps` | freepik | Realistic glass collection | 5551727 |
| `speech-bubbles-002-5388392.eps` | freepik | Glass speech bubbles pack | 5388392 |
| `speech-bubbles-003-198585267.eps` | starline | Neon red chat box | 198585267 |
| `speech-bubbles-004-30086345.eps` | rawpixel.com | Purple balloon | 30086345 |
| `speech-bubbles-005-blue-balloon.eps` | rawpixel.com | Blue balloon | unknown |
| `speech-bubbles-006-green-balloon.eps` | rawpixel.com | Green balloon | unknown |

---

## TWO-TIER ASSET SYSTEM (Architecture Decision)

### Problem
EPS/AI files from Freepik are multi-icon vector sheets (50-100+ icons per file). Video tools (HeyGen, InVideo, Zebracat, CapCut, VEED, Fliki, Synthesia) accept PNG/JPG overlays but cannot open or extract from EPS files.

### Research Findings
- **VistaCreate** is the only affiliate tool that can open EPS — but its API does NOT support EPS import programmatically
- **No n8n node** exists for VistaCreate
- **ConvertAPI / GroupDocs** can convert whole EPS→PNG but cannot extract individual icons from sheets
- **Manual extraction** via Photopea.com or VistaCreate browser is the only path for splitting icon sheets

### Decision: Two Tiers
1. **Tier 1: EPS/AI sheets** = Reference catalog for browsing variety. NOT usable by video tools or n8n workflow.
2. **Tier 2: Individual PNGs** = Production-ready, transparent, single icon. Usable by all video tools.

### Going Forward
- **Prefer individual PNG downloads** from Freepik/Flaticon over EPS sheets
- **Pre-extract PNGs** from existing EPS sheets manually via Photopea/VistaCreate when needed
- **152 brand icons** at `D:/media/assets/icons/` are already individual PNGs — workflow-ready

---

## VIDEO TOOL COMPATIBILITY

| Tool | Role | Accepts | API? | Affiliate? |
|---|---|---|---|---|
| CapCut | TikTok-native editor | PNG, JPG, MP4 | Limited | Yes |
| InVideo | AI video from script | PNG, JPG upload | Yes | Yes |
| HeyGen | AI avatar videos | PNG overlays | Yes | Yes |
| Zebracat | AI video from text | PNG, JPG | Yes | Yes |
| VEED | Browser editor | PNG, JPG, SVG | Yes | Yes |
| Fliki | Text-to-video | PNG | Yes | Yes |
| Synthesia | AI avatar presenter | PNG | Yes | Yes |
| VistaCreate | Design tool (opens EPS) | EPS, PNG, SVG, PSD | Paid API (no EPS import) | Yes |

---

## WORKFLOW ASSET LOOKUP

### How n8n Finds Assets
1. Script generation identifies needed visuals (e.g., "show checkmark for approved")
2. n8n reads `asset-attribution.json` and filters by `tags` + `subfolder` + `icon_name`
3. Matching PNG file path is passed to the video assembly tool
4. Video tool places the icon as an overlay

### Required Fields for Workflow Lookup
- `tags` — searchable keywords (e.g., `["checkmark", "green", "3D"]`)
- `icon_name` — single canonical name (e.g., `"green-checkmark"`) — for extracted PNGs
- `format` — must be `png` for workflow-usable assets
- `scene_affinity` — which scene modes the asset fits (e.g., `["hacker-lab", "all"]`)

---

## DOCUMENTATION UPDATED

| File | Changes |
|---|---|
| `yocoya-fx/STYLE-GUIDE.md` | Added Status column to asset priority table, added Production Pipeline section (file format strategy, video tool compatibility, workflow asset lookup, brand icons) |
| `memory/content-machine.md` | Complete rewrite — repos, API limitations, folder structure with file counts, two-tier asset system, video tool stack, EPS extraction insights, downloaded assets inventory |
| `changelogs/CHANGELOG_v2.0.md` | This file |

---

## NEXT STEPS

- [ ] Download individual PNG icons from Freepik/Flaticon (prefer over EPS sheets)
- [ ] Pre-extract PNGs from existing 11 EPS sheets via Photopea or VistaCreate browser
- [ ] Add `icon_name` and `scene_affinity` fields to existing attribution tracker entries
- [ ] Download remaining Freepik categories (badges, arrows, step indicators — if Essential plan allows)
- [ ] Source SFX/VFX/music from Artlist using shopping lists
- [ ] Build n8n asset lookup node that queries `asset-attribution.json`
- [ ] First test: n8n → pick icon by tag → pass PNG path to InVideo/HeyGen
- [ ] Phase 0 Tech Validation (Blotato API, ElevenLabs es-MX voice quality)

---

*Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>*