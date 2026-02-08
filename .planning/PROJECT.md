# Yocoya Content Machine

## What This Is

An n8n-powered automated pipeline that creates faceless short-form video content about AI topics, featuring a branded AI character persona (Professor Glitch-style). Videos are generated in Spanish with English subtitles, reviewed by a human, and posted to TikTok, Instagram Reels, YouTube Shorts, and X/Twitter via Blotato -- each linking to a specific landing page on yocoya.ai to drive traffic and affiliate conversions.

## Core Value

Automatically generate high-quality, branded AI explainer videos in Spanish that drive consistent traffic from social platforms to yocoya.ai tool pages and articles.

## Requirements

### Validated

(None yet -- ship to validate)

### Active

- [ ] n8n workflow generates video scripts from AI topic inputs
- [ ] AI voiceover generation in Spanish (ElevenLabs or affiliate alternative)
- [ ] AI image/video generation for faceless visual content
- [ ] English subtitle generation for all videos
- [ ] Human review/approval queue before posting
- [ ] Multi-platform posting via Blotato (TikTok, Reels, Shorts, X)
- [ ] Video-specific tracking URLs linking to yocoya.ai landing pages
- [ ] Branded AI character persona with consistent visual identity
- [ ] Content mix: 70% AI term explainers, 30% tool spotlights
- [ ] 3-5 videos per week output cadence

### Out of Scope

- Case study / affiliate walkthrough videos -- defer to v2 after content machine proves reliable
- Long-form video content (YouTube full videos, podcasts) -- short-form only for v1
- Direct platform API posting -- using Blotato for distribution
- English-language versions of videos -- Spanish primary, English subs only
- Automated posting without human review -- human-in-the-loop required

## Context

**Parent project:** Yocoya.ai is an AI news/tools platform targeting Mexican/LATAM audiences. The V2 site rebuild (108 tasks, 10 phases) is designed for maximum clickthrough from tool pages with affiliate CTAs. This content machine is the top-of-funnel driver feeding that V2 site.

**Existing infrastructure:**
- n8n workflow automation running in Docker container on port 5678
- Webflow CMS site (site ID: `68d9932be134e5e066f4a098`) with Noticias and Herramientas collections
- 47+ AI tools already cataloged in Webflow CMS with affiliate links
- Airtable base (`appW3u70zE8C5cda7`) for content tracking
- Existing RSS-to-news pipeline (v2.9.0 workflow) as automation precedent

**Affiliate tool inventory (13 partners):**
- **Voice:** ElevenLabs, LOVO, Murf.ai
- **Video:** InVideo, HeyGen, Kaiber, Zebracat, VEED, OpusClip
- **Images:** Diffus, OpenArt, VistaCreate
- **Social posting:** Blotato

Strategy: Affiliate-first tool selection with quality fallbacks. Using our own affiliate tools in the pipeline creates authentic "dogfooding" content and a compelling meta-narrative.

**Licensed tools (non-affiliate, available for pipeline use):**
- **Midjourney** -- AI image generation (character design, thumbnails, visual assets, b-roll stills)
- **Freepik** -- Stock images, vectors, videos, templates (visual assets, backgrounds)
- **Adobe Creative Cloud** -- Premiere Pro (video editing/templates), After Effects (motion graphics, glitch effects), Photoshop (image editing), Illustrator (character/brand assets)
- **Artlist** -- Royalty-free music, sound effects, and stock footage (background music beds, transition sounds, glitch SFX, b-roll video clips)

These complement the affiliate tools as quality fallbacks or for tasks the affiliate tools can't handle (e.g., Midjourney for Professor Glitch character design, After Effects for intro/outro templates, Artlist for licensed music/SFX beds, Freepik for stock imagery).

**Creative reference:** Professor Glitch TikTok channel -- retro/glitch aesthetic, animated text overlays, approachable AI explainers with rapid cuts, 30-60 second format.

**Traffic loop:** Video (social) --> CTA ("Mas info en yocoya.ai") --> Video-specific landing page on yocoya.ai --> Affiliate CTAs + newsletter signup.

## Constraints

- **Automation platform**: n8n (existing Docker instance) -- all orchestration must run through n8n workflows
- **Language**: Spanish (es-MX) voiceover with English subtitles -- matches yocoya.ai primary locale
- **Human-in-the-loop**: Every video must be reviewed and approved before posting -- no fully autonomous publishing
- **Distribution**: Blotato for multi-platform posting -- no direct platform API integrations in v1
- **Volume**: 3-5 videos per week -- manageable review load while maintaining consistent presence
- **Budget**: Affiliate tool APIs have usage costs -- pipeline must be cost-efficient per video

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Affiliate-first tool selection | Dogfooding our own affiliate tools creates authentic content + referral revenue | -- Pending |
| Blotato for distribution | Single tool for 4 platforms, affiliate partner, reduces API complexity | -- Pending |
| Spanish + English subs | Serves core LATAM audience while catching English speakers via algorithm | -- Pending |
| Human-in-the-loop | Quality control critical for brand -- bad AI content damages trust | -- Pending |
| 70/30 explainer/tool split | Build authority with education first, tool spotlights drive revenue | -- Pending |
| Named AI character persona | Creates recognizable brand identity like Professor Glitch | -- Pending |
| Video-specific landing pages | Higher conversion than generic bio links -- each video has a matching page | -- Pending |

---
*Last updated: 2026-02-08 after initialization*
