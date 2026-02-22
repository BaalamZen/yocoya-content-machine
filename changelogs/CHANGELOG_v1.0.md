# YOCOYA CONTENT MACHINE CHANGELOG v1.0
**Date:** February 8, 2026
**Phase:** Phase 0 - Media Library Foundation & Asset Pipeline Setup
**Status:** COMPLETE - D: drive reorganized, Professor Glitch asset pipeline defined

---

## SESSION SUMMARY

### COMPLETED
- **D:/media Full Reorganization** - 3,247 files across 23GB sorted into project-based structure
- **Professor Glitch Asset Pipeline** - 11 shopping lists created for Artlist & Freepik
- **Migration Script** - Reversible PowerShell script with full move logging
- **SFX/VFX/Music Taxonomy** - Category system built for faceless TikTok production

---

## MEDIA LIBRARY REORGANIZATION

### Problem
- 3,237+ files dumped across 12 flat folders with no project separation
- Videos mixed in audio folder, spreadsheets mixed in branding folder
- AI-generated images from 5+ sources (Midjourney, Nano Banana, Freepik, Vecteezy, ChatGPT) all in one `images/` dump folder
- No distinction between stock assets, branded assets, and creative art
- Business docs (CSVs, XLSX, DOCX) buried alongside visual assets

### Solution
Automated PowerShell migration script (`D:/media/_reorganize.ps1`) with:
- Full dry-run mode for preview before execution
- Every move logged to `D:/media/_migration-log.txt` for reversibility
- Pattern-based file sorting (by filename prefix, extension, content type)
- Empty directory cleanup after migration

### New Structure

```
D:/media/
├── _archive/                          # Safety holding area
├── assets/                            # 150 files - shared, non-project
│   ├── backgrounds/                   # Horizontal & vertical BGs
│   ├── fonts/                         # (future expansion)
│   ├── icons/                         # 3D web icons + tool logos (80 files)
│   ├── stock-images/                  # By source: adobe, freepik, vecteezy
│   └── stock-footage/                 # Getty licensed clips (17 files)
├── audio/                             # 38 files
│   ├── music/
│   │   ├── high-energy/              # Electronic, cyberpunk, trap
│   │   ├── chill-beat/               # Lo-fi, boom bap (22 existing tracks)
│   │   └── ambient/                  # 432Hz/528Hz/963Hz meditation (9 tracks)
│   ├── sfx/
│   │   ├── glitch/                   # Digital corruption, bitcrush
│   │   ├── whoosh/                   # Transitions, swooshes
│   │   ├── ui/                       # Clicks, pops, notifications
│   │   ├── impact/                   # Hits, bass drops, risers
│   │   └── ambient/                  # Drones, tension, hums
│   └── raw/                           # Raw recordings
├── video/                             # 32 files
│   ├── footage/                       # Generated & personal footage (10 files)
│   ├── vfx/
│   │   ├── glitch/                   # RGB split, scan lines (3 files)
│   │   ├── overlays/                 # Energy, arcs, orbs, smoke (17 files)
│   │   ├── text-reveals/            # Animated text templates
│   │   └── backgrounds/             # Looping BGs for video
│   └── output/                        # Final renders
├── projects/                          # 2,805 files
│   ├── yocoya/                        # 1,183 files
│   │   ├── branding/                 # Logos, badges, holo text, web icons
│   │   ├── website/                  # Hero images, banners, web assets
│   │   ├── youtube/                  # YT thumbnails & backgrounds
│   │   └── docs/                     # CSVs, XLSX, PDFs, business docs
│   ├── kai/                           # 92 files - branding + avatars
│   ├── baalam/                        # 23 files - branding
│   ├── etsy/                          # 1,366 files - clothing, mockups, docs
│   └── avatars/                       # 141 files - shared characters
│       ├── micah/ nayeli/ tre/ willow/ personalities/ locations/
├── creative/                          # 222 files - themed art
│   ├── abstract/ buddha-zen/ fantasy/ japanese-backdrops/
│   ├── lotus-flowers/ mandalas/ nature/ dossiers/
│   ├── text-overlays/ locations/
│   └── misc/                          # 7 uncategorized files
```

### Migration Stats
| Metric | Value |
|---|---|
| Total files moved | 3,247 |
| Errors | 0 |
| Old directories cleaned | 12 |
| New directories created | 60+ |
| Script execution time | ~1 second |

---

## PROFESSOR GLITCH ASSET PIPELINE

### Context
Faceless TikTok channel ("Professor Glitch") needs a production-ready asset library for short-form video creation. Current SFX folder was completely empty, music was all ambient/meditation (wrong energy for TikTok).

### Shopping Lists Created
11 `_SHOPPING-LIST.md` files deployed into target folders, each containing:
- Artlist pack links and search terms
- Freepik search terms
- Categorized download checklists with target quantities
- Naming conventions for consistency

| Shopping List | Target Folder | Items to Source |
|---|---|---|
| sfx-glitch.md | `audio/sfx/glitch/` | ~20-30 digital glitch hits |
| sfx-whoosh.md | `audio/sfx/whoosh/` | ~20 transition swooshes |
| sfx-ui.md | `audio/sfx/ui/` | ~30 clicks, pops, typing |
| sfx-impact.md | `audio/sfx/impact/` | ~20 hits, bass drops, risers |
| sfx-ambient.md | `audio/sfx/ambient/` | ~10-15 drones, hums |
| music-high-energy.md | `audio/music/high-energy/` | ~10-15 cyberpunk/trap beats |
| music-chill-beat.md | `audio/music/chill-beat/` | ~8-10 lo-fi/boom bap |
| vfx-glitch.md | `video/vfx/glitch/` | ~15 RGB split, scan lines |
| vfx-overlays.md | `video/vfx/overlays/` | ~15-20 light leaks, HUD |
| vfx-text-reveals.md | `video/vfx/text-reveals/` | ~10-15 animated text templates |
| vfx-backgrounds.md | `video/vfx/backgrounds/` | ~10-15 looping BGs |

### Existing Assets Sorted
- 17 VFX overlays (energy spheres, electric arcs, orbs, smoke) → `video/vfx/overlays/`
- 2 static/noise VFX → `video/vfx/glitch/`
- 9 frequency tracks (432Hz, 528Hz, 963Hz) → `audio/music/ambient/`
- 22 licensed chill tracks → `audio/music/chill-beat/`

---

## KEY DECISIONS

1. **Project-first organization** — AI-generated images stay with their project (KAI logos in `projects/kai/`) rather than grouped by AI source
2. **Script-based migration** — reversible, logged, reviewable before execution
3. **Shopping lists in-folder** — each `_SHOPPING-LIST.md` lives inside its target folder so you see what's needed when you open it
4. **Empty folders kept** — fonts, raw, output, sfx subfolders preserved as placeholders for future expansion
5. **Nano Banana free tier** — chosen over local generation (ComfyUI/FLUX) for image needs, despite having RTX 5070 available

---

## FILES GENERATED

| File | Location |
|---|---|
| `_reorganize.ps1` | `D:/media/` |
| `_migration-log.txt` | `D:/media/` |
| `sfx-glitch.md` | `D:/media/_shopping-lists/` |
| `sfx-whoosh.md` | `D:/media/_shopping-lists/` |
| `sfx-ui.md` | `D:/media/_shopping-lists/` |
| `sfx-impact.md` | `D:/media/_shopping-lists/` |
| `sfx-ambient.md` | `D:/media/_shopping-lists/` |
| `music-high-energy.md` | `D:/media/_shopping-lists/` |
| `music-chill-beat.md` | `D:/media/_shopping-lists/` |
| `vfx-glitch.md` | `D:/media/_shopping-lists/` |
| `vfx-overlays.md` | `D:/media/_shopping-lists/` |
| `vfx-text-reveals.md` | `D:/media/_shopping-lists/` |
| `vfx-backgrounds.md` | `D:/media/_shopping-lists/` |

---

## NAMING CONVENTION & BULK RENAME

### Problem
~397 files (10.4%) had unusable names after reorganization:
- AI prompt filenames 200+ characters long ("Elongated glass mandala, oval jewel-like structure...")
- UUID hashes ("0e4f9007-ed58-4cbc-bcb9-5d4fbc35b580.png")
- Midjourney IDs ("u2675149154_A_hyper-realistic_3D_coin...")
- Nano Banana timestamps ("nano-banana-2025-09-05T19-52-41.png")
- ChatGPT/Gemini timestamps ("Generated Image September 09, 2025 - 11_35PM.png")
- URL-encoded characters ("KAI%2520Logo%2520-%2520Up_upscayl_4x.png")
- Inconsistent casing, spaces, underscores

### Solution
1. **Naming convention standard** (`D:/media/_NAMING-CONVENTION.md`) — permanent guide:
   - All lowercase, hyphens only, max 60 chars
   - Format: `[context]-[descriptor]-[##].[ext]`
   - Per-folder patterns (e.g., `kai-coin-01.png`, `lotus-neon-blue-crystal.png`)
   - Strip all AI source prefixes, UUIDs, timestamps

2. **Bulk rename script** (`D:/media/_rename-files.ps1`) — same dry-run/logged pattern:
   - 267 files renamed across 11 folders
   - 0 errors
   - Full old → new mapping in `D:/media/_rename-log.txt` (preserves Getty/AdobeStock IDs for licensing)

### Rename Stats
| Metric | Value |
|---|---|
| Files renamed | 267 |
| Errors | 0 |
| Midjourney prefix remaining | 0 |
| Nano Banana prefix remaining | 0 |
| Spaces in filenames remaining | 0 (in targeted folders) |

### Folders Cleaned
| Folder | Before | After |
|---|---|---|
| `projects/kai/branding/` | 62.9% bad names | Clean |
| `creative/lotus-flowers/` | 89.3% bad names | Clean |
| `creative/mandalas/` | 100% bad names | Clean |
| `creative/buddha-zen/` | 39.5% bad names | Clean |
| `assets/stock-images/vecteezy/` | 100% bad names | Clean |
| `assets/icons/3d-web-icons/` | 100% bad names | Clean |
| `assets/stock-footage/` | 100% bad names | Clean |
| `projects/yocoya/website/` | 65%+ bad names | Clean |
| `creative/abstract/` | 32% bad names | Clean |
| `creative/misc/` | 100% bad names | Clean |

### Skipped (for manual review)
- **Dossiers** (60 ChatGPT files) — need visual context to name
- **Avatars** (72 UUID files) — need visual context to name
- **Etsy** (1,366 files) — separate project

### Files Generated
| File | Purpose |
|---|---|
| `_NAMING-CONVENTION.md` | Permanent naming rules & per-folder patterns |
| `_rename-files.ps1` | Reusable rename script |
| `_rename-log.txt` | Full old→new mapping for license tracking |

---

## ASSET INVENTORY & FOLDER STRUCTURE (February 20, 2026)

### Context
Created a searchable asset inventory CSV and matching folder structure on Desktop (`yocoya-fx/`) to serve as a staging area for downloading assets from Artlist and Freepik before importing into the production media library.

### Asset Inventory CSV
- **File:** `Desktop/yocoya-fx/asset-inventory.csv`
- **59 asset types** across 7 categories, each with 5 optimized search terms
- **Platform column** indicates whether to search Artlist or Freepik
- Includes expanded **Glitch AI background** entry with 20 search terms (4 rows)

| Category | Assets | Platform |
|---|---|---|
| SFX | 13 types (whooshes, transitions, impacts, glitch, UI clicks, typing, notifications, power up, error, beeps, ambient, risers, pop/reveal) | Artlist |
| Music | 5 types (high-energy intro, lofi chill, tech electronic, explainer, outro) | Artlist |
| VFX | 12 types (glitch overlay, scan lines, text reveal, lower thirds, transition wipes, particles, light leaks, digital rain, HUD, neon glow, CTA/subscribe, logo reveal) | Artlist |
| Backgrounds | 11 types (abstract tech, neural network, circuit board, data viz, server/datacenter, robot/AI, screen mockup, gradients, earth/global, hologram, **glitch AI**) | Artlist |
| Graphics | 10 types (AI icons, tool icons, emoji, arrows, badges, versus, step indicators, speech bubbles, checkmarks, price tags) | Freepik |
| Brand | 5 types (character avatar, teal frames, watermarks, intro/outro slates, thumbnails) | Freepik |

### Folder Structure Created
62 directories matching the CSV inventory:

```
Desktop/yocoya-fx/
├── asset-inventory.csv
├── sfx/           (13 subfolders)
├── music/         (5 subfolders)
├── vfx/           (12 subfolders)
├── backgrounds/   (11 subfolders, includes glitch-ai/)
├── graphics/      (10 subfolders)
└── brand/         (5 subfolders)
```

---

## FREEPIK MCP SERVER & ATTRIBUTION TRACKER (February 22, 2026)

### Context
Built a local Freepik API MCP server to automate asset search, download, and attribution tracking. Downloads go directly into the `yocoya-fx/` staging folder with auto-incrementing filenames. Attribution is logged automatically on every download.

### Freepik MCP Server
- **Location:** `yocoya.ai/mcp-servers/freepik/server.py`
- **Pattern:** Same as webflow-data — FastMCP + httpx, runs via `uv run`
- **Config:** `.mcp.json` with `FREEPIK_API_KEY` env var
- **9 tools:**

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

### Naming Convention
Files saved as `{subfolder}-{###}-{resource_id}.{ext}` with auto-incrementing sequence per folder.
- `###` = 3-digit sequence (001, 002, ...)
- `resource_id` = Freepik ID (ties back to license/re-download)
- Examples: `ai-icons-001-390228701.eps`, `thumbnails-003-87654321.psd`

### Attribution Tracker
- **File:** `yocoya-fx/asset-attribution.json`
- **Auto-populated on every `download_and_track` call**
- **Fields per record:**

| Field | Description |
|---|---|
| `id` | Auto-increment ID |
| `filename` | Local filename (naming convention) |
| `source_platform` | "Freepik" |
| `source_url` | Full URL on Freepik |
| `resource_id` | Freepik resource ID |
| `freepik_title` | Original title on Freepik |
| `author` | Author name |
| `license_type` | Premium or Freemium |
| `is_ai_generated` | Boolean from Freepik metadata |
| `attribution_required` | Boolean (Freemium=yes, Premium=no) |
| `attribution` | Formatted: "Designed by X on Freepik" |
| `category` | Top-level folder (graphics, brand, etc.) |
| `subfolder` | Subfolder name (ai-icons, thumbnails, etc.) |
| `format` | File extension (eps, svg, png, psd) |
| `dimensions` | Width x height |
| `search_term` | Query that found the asset |
| `used_for` | Intended purpose |
| `used_in` | Fill in later when asset appears in a video/page |
| `tags` | Comma-separated tags |
| `local_path` | Full local path |
| `downloaded_at` | ISO timestamp |

### Platform Split (from asset-inventory.csv)
- **Freepik (MCP-automated):** Graphics (10 types) + Brand (5 types) = 15 asset categories
- **Artlist (manual):** SFX (13) + Music (5) + VFX (12) + Backgrounds (15) = 45 asset categories

---

## NEXT STEPS

- [ ] Download 15 Freepik asset categories via MCP (Graphics + Brand)
- [ ] Download SFX from Artlist using shopping lists (start with glitch + whoosh + impact)
- [ ] Download VFX overlays from Artlist (glitch overlays + text reveals)
- [ ] Download glitch AI backgrounds from Artlist (20 search terms ready)
- [ ] Source 10-15 high-energy music tracks for TikTok
- [ ] Create Professor Glitch intro/outro template using new assets
- [ ] Connect asset pipeline to n8n video production workflow
- [ ] First test video with full SFX/VFX/music stack
- [ ] Manually review & rename dossier files (60 files)
- [ ] Manually review & rename avatar UUID files (72 files)

---

*Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>*
