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

Visual assets live in `~/OneDrive/Desktop/yocoya-fx/` (staging) and `D:/media/` (production).

**Two-tier system:**
- **EPS/AI sheets** -- reference catalog for browsing icon variety (not usable by video tools)
- **Individual PNGs** -- production-ready, transparent, single icon (workflow-ready)

The n8n workflow finds assets by querying `yocoya-fx/asset-attribution.json`:
1. Script generation identifies needed visuals (e.g., "show checkmark")
2. n8n filters by `tags` + `subfolder` + `icon_name`
3. Matching PNG path is passed to the video assembly tool as overlay

See `yocoya-fx/STYLE-GUIDE.md` for icon styles, scene modes, and Freepik search keywords.

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
