# Yocoya Content Machine

n8n-powered automated pipeline for faceless AI explainer videos in Spanish -- drives traffic to [yocoya.ai](https://yocoya.ai).

## What It Does

Automatically generates short-form video content (30-60s) about AI topics, featuring a branded character persona ("Professor Glitch" style). Videos are created in Spanish with English subtitles, reviewed by a human, and posted to TikTok, Instagram Reels, YouTube Shorts, and X/Twitter -- each linking to a specific yocoya.ai landing page for affiliate conversions.

## Content Mix

- **70% AI Term Explainers** -- LLMs, RAG, Generative AI, etc.
- **30% Tool Spotlights** -- sourced from yocoya.ai's 97+ tool catalog

## Pipeline

```
Topic Selection → Script Generation (LLM) → Voice (ElevenLabs es-MX)
→ Visual Assets → Video Assembly (9:16 vertical) → Subtitles (ES + EN)
→ Human Review (Airtable) → Multi-Platform Posting (Blotato)
→ Analytics & Tracking (UTM → yocoya.ai)
```

## Tech Stack

- **Orchestration:** n8n (Docker)
- **Script Writing:** Claude / GPT via n8n AI nodes
- **Voice:** ElevenLabs (es-MX) -- affiliate partner
- **Video:** HeyGen / InVideo / Zebracat -- affiliate partners
- **Subtitles:** Auto-generated ES + EN
- **Posting:** Blotato (TikTok, IG Reels, YT Shorts, X) -- affiliate partner
- **Review Queue:** Airtable
- **Content Source:** Webflow CMS (Herramientas collection) + Airtable RSS feeds
- **Tracking:** UTM params per video → yocoya.ai landing pages

## Asset Pipeline

**Master asset library:** `D:/media/` -- production-ready, fully standardized (4,621 files renamed, 100% naming-convention compliant as of 2026-02-25).

```text
D:/media/
├── assets/         # Backgrounds, fonts, icons (152 tool logos), stock images/footage
├── audio/          # Music (chill-beat, high-energy, ambient), SFX (glitch, whoosh, UI, impact)
├── creative/       # AI-generated art: lotus, mandalas, buddha-zen, abstract, fantasy, nature
├── projects/       # Per-brand: yocoya (branding, youtube, fx, affiliate-partners), kai, baalam, etsy
├── video/          # Footage, VFX (glitch, overlays, text-reveals, looping backgrounds), output
├── _NAMING-CONVENTION.md   # Rules: lowercase, hyphens, max 60 chars, [context]-[descriptor]-[##].[ext]
├── _shopping-lists/        # 11 Artlist/Freepik guides for filling SFX/VFX/music gaps
└── _rename-log.txt         # License tracking: old→new filename mapping (never delete)
```

**Staging area:** `~/OneDrive/Desktop/yocoya-fx/` -- for downloading/organizing new assets before importing to `D:/media/`.

**Two-tier system:**
- **EPS/AI sheets** -- reference catalog for browsing icon variety (not usable by video tools)
- **Individual PNGs** -- production-ready, transparent, single icon (workflow-ready)

The n8n workflow finds assets by querying `yocoya-fx/asset-attribution.json`:
1. Script generation identifies needed visuals (e.g., "show checkmark")
2. n8n filters by `tags` + `subfolder` + `icon_name`
3. Matching PNG path from `D:/media/` is passed to the video assembly tool as overlay

See `D:/media/_NAMING-CONVENTION.md` for naming rules and per-folder patterns.

## Affiliate Tools in Pipeline

We dogfood our own affiliate partners wherever possible:

| Stage | Affiliate Tool | Fallback |
|-------|---------------|----------|
| Voice | ElevenLabs, LOVO, Murf.ai | Azure TTS |
| Video | InVideo, HeyGen, Kaiber, Zebracat, VEED, OpusClip | FFmpeg + stock |
| Images | Diffus, OpenArt, VistaCreate | Pexels / Pixabay |
| Posting | Blotato | Direct platform APIs |

## Roadmap

| Phase | Goal | Status |
|-------|------|--------|
| 0. Tech Validation | Verify Blotato, ElevenLabs es-MX, avatar creation | Not started |
| 1. Script Generation | n8n workflow generates scripts from topics | Not started |
| 2. Audio Pipeline | Spanish TTS with character voice | Not started |
| 3. Video Assembly | Combine audio + visuals + subtitles | Not started |
| 4. Character & Brand | Professor Glitch persona + Yocoya branding | Not started |
| 5. Review Queue | Human approval gate via Airtable | Not started |
| 6. Distribution | 4-platform posting via Blotato | Not started |
| 7. Content Sourcing | Auto-select topics from CMS + news feeds | Not started |
| 8. Analytics & Tracking | UTM tracking + performance metrics | Not started |
| 9. Error Handling | Logging, alerts, retry logic | Not started |

See `.planning/` for detailed requirements, research, and roadmap.

## Related

- [yocoya.ai](https://yocoya.ai) -- the destination site (Webflow CMS)
- [yocoya.ai repo](https://github.com/BaalamZen/yocoya.ai) -- site codebase + V2 rebuild plan
