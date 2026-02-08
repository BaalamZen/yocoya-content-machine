# Technology Stack

**Project:** Yocoya Content Machine
**Domain:** Automated Faceless Short-Form Video Content Pipeline
**Researched:** 2026-02-08
**Overall Confidence:** MEDIUM

## Recommended Stack

### Core Orchestration
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| n8n | 1.x+ | Workflow automation orchestrator | Already deployed in Docker, visual workflow builder, extensive integrations, webhook support, self-hosted control. Proven in existing Yocoya news pipeline. |
| Docker | 20.x+ | Container runtime | Existing infrastructure, ensures consistent n8n environment, simplifies deployment. |
| Airtable | API v0.1+ | Content queue and state management | Already integrated, acts as central database for workflow state, review queue, publishing schedule. |

**Confidence:** HIGH - n8n and Airtable already proven in production for yocoya.ai news pipeline.

### AI Voice Generation (Primary)
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| ElevenLabs | API v1 | Text-to-speech voice generation | AFFILIATE TOOL. Industry leader in natural voice quality, excellent Spanish support, voice cloning for Professor Glitch character, API-first design, proven at scale. Offers multilingual v2 models with cross-language voice cloning. |

**Alternatives (Fallback Order):**
1. **LOVO** (AFFILIATE) - Good voice quality, Spanish support, lower cost than ElevenLabs
2. **Murf.ai** (AFFILIATE) - Professional voices, decent Spanish, emphasis on business use cases

**Confidence:** MEDIUM - Training data through Jan 2025. ElevenLabs was market leader but pricing and API capabilities need 2026 verification.

### AI Video Generation (Primary)
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| HeyGen | API v1+ | Avatar-based video generation | AFFILIATE TOOL. Best for creating consistent Professor Glitch character, supports custom avatars, excellent lip-sync quality, API access, Spanish language support. Purpose-built for faceless talking-head content. |

**Alternatives (Fallback Order):**
1. **InVideo AI** (AFFILIATE) - Script-to-video automation, stock footage library, text-to-video
2. **VEED** (AFFILIATE) - Strong editing API, subtitle generation, template-based video creation
3. **Kaiber** (AFFILIATE) - AI art-to-video, creative transformations (less suitable for informational content)

**Confidence:** LOW - HeyGen was emerging as leader but 2026 API capabilities need verification. InVideo may have stronger automation features.

### AI Image/B-Roll Generation
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| OpenArt | API/Web | AI-generated imagery for B-roll | AFFILIATE TOOL. Stable Diffusion models, style consistency, prompt engineering support. Good for tech/AI themed imagery. |
| Diffus | API | Background visuals and effects | AFFILIATE TOOL. Specialized in diffusion models for creative effects. |

**Alternatives:**
- **VistaCreate** (AFFILIATE) - Template-based design, less AI-native but easier consistency
- Stock footage from **InVideo** library when using their video generation

**Confidence:** LOW - Image generation landscape evolving rapidly. OpenAI DALL-E, Midjourney, Stable Diffusion ecosystem changed significantly. Need 2026 verification.

### Video Editing & Post-Production
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| VEED | API | Subtitle generation, video editing | AFFILIATE TOOL. Strong API for programmatic editing, auto-subtitle generation (Spanish + English), rendering pipeline. Better suited for post-processing than primary generation. |
| OpusClip | API | Short-form clip optimization | AFFILIATE TOOL. Specializes in converting long-form to short-form, virality scoring. May not be needed if generating shorts from scratch, but useful for content repurposing. |

**Confidence:** MEDIUM - VEED had strong API offerings. OpusClip's fit for this use case questionable (designed for clipping existing content, not generating new).

### Social Media Distribution
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Blotato | API | Multi-platform posting (TikTok, Instagram, YouTube, X) | AFFILIATE TOOL. Purpose-built for short-form video distribution across multiple platforms. Single API for all targets. If API robust, ideal for automation. |

**Fallback:**
- Direct platform APIs (TikTok, Instagram Graph API, YouTube Data API, X API v2)
- **Buffer** or **Hootsuite** (not affiliates, but proven reliability)

**Confidence:** LOW - Blotato appears to be newer service (less established than Buffer/Hootsuite). Must verify: API maturity, rate limits, platform support reliability, pricing. CRITICAL RESEARCH FLAG.

### Script Generation
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| OpenAI GPT-4 | API | AI news script writing | Industry standard for long-form text generation, function calling for structured output, excellent Spanish, JSON mode for structured scripts. |
| Anthropic Claude | API | Alternative/fallback script generation | Strong reasoning, long context for research, excellent at following style guides. |

**Confidence:** HIGH - Both providers well-established with stable APIs.

### URL Shortening & Analytics
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Bitly | API v4 | Short URLs with analytics | Industry standard, reliable API, campaign tracking, click analytics. Tracks affiliate conversions via landing page traffic. |
| Rebrandly | API | Custom domain short URLs | Better branding (yoco.ya URLs), more expensive but professional. |

**Confidence:** HIGH - Established services with stable APIs.

### Supporting Infrastructure
| Technology | Version | Purpose | Why |
|------------|---------|---------|-----|
| Webflow CMS | API v2 | Landing page content management | Already deployed for yocoya.ai, hosts landing pages linked from videos. |
| Node.js | 18.x LTS+ | n8n custom functions | n8n's execution environment, used for complex transforms in workflows. |

**Confidence:** HIGH - Existing production infrastructure.

## Affiliate Tool Evaluation

### Tier 1: Primary Recommendations (Strong Fit)
| Tool | Category | Fit Score | Why |
|------|----------|-----------|-----|
| **ElevenLabs** | Voice | 9/10 | Best voice quality, Spanish support, character voice cloning, API-first. Only concern: pricing at scale. |
| **HeyGen** | Video | 8/10 | Avatar consistency, lip-sync quality, talking-head format matches Professor Glitch concept. Needs API capability verification. |
| **VEED** | Post-Production | 8/10 | Strong subtitle API (bilingual requirement), editing automation. Better as post-processor than primary generator. |
| **Blotato** | Distribution | 7/10 | Purpose-built for short-form multi-platform. CRITICAL: verify API maturity and reliability. High risk if unproven. |

### Tier 2: Viable Alternatives (Fallback/Specialized)
| Tool | Category | Fit Score | Why |
|------|----------|-----------|-----|
| **InVideo** | Video | 7/10 | Strong script-to-video automation, stock library. Less character-consistent than HeyGen but more autonomous. |
| **LOVO** | Voice | 7/10 | Cost-effective ElevenLabs alternative, good Spanish support. |
| **Murf.ai** | Voice | 6/10 | Professional quality, emphasis on business content. May lack character voice nuance. |
| **OpenArt** | Images | 6/10 | Good for AI-themed B-roll. Image generation landscape very competitive. |
| **Diffus** | Images | 6/10 | Creative effects but unclear API maturity. |

### Tier 3: Poor Fit (Not Recommended for This Pipeline)
| Tool | Category | Fit Score | Why Not |
|------|----------|-----------|--------|
| **OpusClip** | Editing | 4/10 | Designed for clipping existing long-form content, not generating shorts from scratch. Virality scoring interesting but not core need. |
| **Zebracat** | Video | 3/10 | No clear differentiator vs InVideo/HeyGen. Less established. Avoid unless has unique feature. |
| **Kaiber** | Video | 3/10 | Artistic/creative transformations. Poor fit for informational news content requiring clarity and consistency. |
| **VistaCreate** | Images | 4/10 | Template-based design tool, not AI-native. Manual workflow, weak automation. |

### Not Evaluated (Missing Context)
- Tool details unavailable in training data or unclear positioning

## Recommended Primary Stack

**For MVP (Phase 1):**
```
Orchestration: n8n (existing)
Database: Airtable (existing)
Script: OpenAI GPT-4
Voice: ElevenLabs (affiliate)
Video: HeyGen (affiliate)
Subtitles: VEED API (affiliate)
Distribution: Blotato (affiliate) OR direct APIs (if Blotato immature)
URLs: Bitly
```

**Rationale:** Maximize affiliate revenue while using proven services. HeyGen + ElevenLabs = consistent Professor Glitch character. VEED handles bilingual subtitles. Blotato is risk point (verify before committing).

**For Scale (Phase 2+):**
- Add **LOVO** as voice fallback (cost optimization)
- Add **InVideo** as video fallback (if HeyGen API limits hit)
- Consider **Rebrandly** for branded short URLs

## Alternatives Considered

| Category | Recommended | Alternative | When to Use Alternative |
|----------|-------------|-------------|-------------------------|
| Voice | ElevenLabs | LOVO | Cost constraints, or ElevenLabs API limits reached |
| Voice | ElevenLabs | Murf.ai | Need enterprise features, bulk licensing |
| Video | HeyGen | InVideo | Avatar consistency less important, prefer stock footage style |
| Video | HeyGen | Direct editing (VEED + assets) | Maximum control, HeyGen doesn't fit style |
| Distribution | Blotato | Direct Platform APIs | Blotato API unreliable or missing features |
| Images | OpenArt | Midjourney API | Need highest quality (if API access available) |
| Images | OpenArt | Stock footage from InVideo | Consistency more important than AI generation |

## What NOT to Use

| Avoid | Why | Use Instead |
|-------|-----|-------------|
| **Zebracat** | No clear value proposition vs established tools. Less proven. | InVideo, HeyGen, or VEED |
| **Kaiber** | Artistic style unsuitable for news/informational content. Inconsistent output. | HeyGen for consistency, InVideo for automation |
| **VistaCreate** | Not AI-native, manual workflows, weak automation. Template-based. | OpenArt, Diffus, or stock libraries |
| **OpusClip** | Wrong use case (clips existing content, not generates new). | VEED for editing, HeyGen/InVideo for generation |
| **Make.com** (not affiliate) | Competitor to n8n. No reason to switch from working n8n setup. | n8n (already deployed) |
| **Zapier** (not affiliate) | More expensive than n8n, less flexible, not self-hosted. | n8n (already deployed) |

## Stack Patterns by Use Case

### Pattern 1: Avatar-Based (Professor Glitch Character)
**When:** Consistent character persona, talking-head style
**Stack:**
- Script: GPT-4
- Voice: ElevenLabs (custom voice)
- Video: HeyGen (custom avatar)
- Subtitles: VEED
- Post: Blotato

**Why:** Character consistency paramount. HeyGen excels at avatar-based content with lip-sync.

### Pattern 2: Stock Footage + Voiceover
**When:** No character needed, variety of visuals, faster production
**Stack:**
- Script: GPT-4
- Voice: ElevenLabs
- Video: InVideo (script-to-video with stock)
- Subtitles: VEED or InVideo built-in
- Post: Blotato

**Why:** InVideo automates stock footage selection and timing. Faster but less branded.

### Pattern 3: Cost-Optimized
**When:** High volume, budget constraints
**Stack:**
- Script: GPT-4 (or Claude for cost)
- Voice: LOVO (cheaper alternative)
- Video: InVideo (bulk pricing)
- Subtitles: VEED
- Post: Direct APIs (avoid Blotato fees)

**Why:** Reduces per-video cost for scale. Sacrifices some quality and consistency.

## Installation & Setup

### n8n Workflow Dependencies
```bash
# n8n already running in Docker
# Add community nodes if needed:
docker exec n8n npm install n8n-nodes-elevenlabs
docker exec n8n npm install n8n-nodes-openai

# Restart n8n
docker restart n8n
```

### API Keys Required
```bash
# Voice
ELEVENLABS_API_KEY=
LOVO_API_KEY=          # fallback

# Video
HEYGEN_API_KEY=
INVIDEO_API_KEY=       # fallback
VEED_API_KEY=

# Images
OPENART_API_KEY=
DIFFUS_API_KEY=

# Script
OPENAI_API_KEY=
ANTHROPIC_API_KEY=     # fallback

# Distribution
BLOTATO_API_KEY=
# OR direct:
TIKTOK_API_KEY=
INSTAGRAM_API_KEY=
YOUTUBE_API_KEY=
X_API_KEY=

# URLs
BITLY_API_KEY=

# Existing
AIRTABLE_API_KEY=      # already configured
WEBFLOW_API_KEY=       # already configured
```

### n8n Nodes to Install
- HTTP Request (built-in)
- Airtable (built-in)
- Code (built-in, for Node.js transforms)
- Webhook (built-in, for callbacks)
- OpenAI (community node or HTTP)
- ElevenLabs (community node or HTTP)
- Custom HTTP nodes for each API

## Version Compatibility Notes

**n8n Version Considerations:**
- n8n 1.x+ required for improved Code node and error handling
- Community nodes may lag behind n8n releases
- Prefer HTTP Request node over community nodes for API stability

**Node.js in n8n:**
- n8n uses Node.js 18.x LTS
- Custom Code nodes execute in this environment
- Avoid Node.js 20+ features not in LTS

**API Version Pinning:**
- Pin API versions in HTTP requests (e.g., `/v1/`, `/v2/`)
- Monitor for deprecation notices
- ElevenLabs, HeyGen, OpenAI all have versioned APIs

## Critical Research Gaps (Must Verify Before Build)

### HIGH PRIORITY
1. **Blotato API Maturity** - Is API production-ready? Rate limits? Platform coverage? If immature, need direct API integration plan.
2. **HeyGen API Availability** - Does API support custom avatars? Pricing model? Spanish language support?
3. **InVideo API Capabilities** - Script-to-video automation via API? Or just editing?
4. **ElevenLabs 2026 Pricing** - Voice generation costs at scale (100-1000 videos/month)?

### MEDIUM PRIORITY
5. **VEED Subtitle API** - Bilingual subtitle generation (Spanish + English simultaneously)?
6. **OpenArt/Diffus API Access** - Public API or platform-only?
7. **Affiliate Program Terms** - Commission rates, tracking, payout terms for all 13 tools
8. **Platform API Limits** - TikTok, Instagram, YouTube posting limits and requirements

### LOW PRIORITY
9. **LOVO/Murf.ai API** - Fallback voice providers' current offerings
10. **Zebracat Positioning** - Any unique features that justify inclusion?

## Sources

**Training Data (January 2025):**
- ElevenLabs documentation and API capabilities
- HeyGen platform features and use cases
- n8n workflow automation patterns
- Short-form video automation landscape
- Social media platform APIs

**Existing Infrastructure (HIGH confidence):**
- n8n deployment at C:\Users\baala\Dev\yocoya\yocoya.ai
- Airtable base appW3u70zE8C5cda7
- Webflow site 68d9932be134e5e066f4a098

**CONFIDENCE ASSESSMENT:**
- Core orchestration (n8n, Airtable): HIGH - proven in production
- Voice generation (ElevenLabs): MEDIUM - training data current through Jan 2025
- Video generation (HeyGen, InVideo): LOW - rapidly evolving space, need 2026 verification
- Distribution (Blotato): LOW - unclear API maturity, CRITICAL research gap
- Script generation (OpenAI, Anthropic): HIGH - stable, established APIs
- Infrastructure: HIGH - existing production environment

---
**Stack research for:** Yocoya Content Machine - Automated Faceless Video Pipeline
**Researched:** 2026-02-08
**Next:** Verify 2026 API capabilities for Blotato, HeyGen, InVideo, ElevenLabs pricing
