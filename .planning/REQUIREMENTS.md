# Requirements: Yocoya Content Machine

**Defined:** 2026-02-08
**Core Value:** Automatically generate high-quality, branded AI explainer videos in Spanish that drive consistent traffic from social platforms to yocoya.ai tool pages and articles.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### Tech Validation

- [ ] **VALID-01**: ElevenLabs es-MX voice generates natural-sounding 60s audio approved by native Mexican speaker
- [ ] **VALID-02**: Blotato successfully posts a test video to all 4 platforms (TikTok, Instagram Reels, YouTube Shorts, X)
- [ ] **VALID-03**: HeyGen or alternative can create a custom avatar character for the Professor Glitch persona

### Script Generation

- [ ] **SCRPT-01**: n8n workflow generates a structured video script (hook, body, CTA) from a topic input using LLM (Claude/GPT)
- [ ] **SCRPT-02**: AI term explainer template produces scripts for concepts like LLMs, RAG, Generative AI with consistent format
- [ ] **SCRPT-03**: Tool spotlight template produces scripts from Webflow CMS tool data (name, description, features, pricing, affiliate link)
- [ ] **SCRPT-04**: Scripts include platform-specific CTA directing viewers to the matching yocoya.ai landing page

### Voice & Audio

- [ ] **VOICE-01**: TTS generates Spanish (es-MX) voiceover from script using ElevenLabs or affiliate alternative
- [ ] **VOICE-02**: Consistent named voice is used across all videos for character persona recognition
- [ ] **VOICE-03**: Audio output is production-ready quality (no artifacts, natural pacing, correct pronunciation of AI terms)

### Visual Generation

- [ ] **VISUL-01**: Pipeline generates visual assets (stock footage, images, or AI-generated) mapped to script sections
- [ ] **VISUL-02**: Visual style matches Professor Glitch aesthetic (retro/glitch, animated text overlays, rapid cuts)
- [ ] **VISUL-03**: Brand consistency maintained across all videos (color palette, intro/outro, character placement)

### Video Assembly

- [ ] **VIDEO-01**: Pipeline combines audio, visuals, and subtitles into a final video file (9:16 vertical, 30-60 sec, MP4)
- [ ] **VIDEO-02**: Spanish subtitles are auto-generated and synced to voiceover
- [ ] **VIDEO-03**: English subtitles are generated as secondary track/overlay
- [ ] **VIDEO-04**: Video meets platform specs for all 4 targets (resolution, codec, file size, safe zones)

### Character & Brand

- [ ] **BRAND-01**: Named AI character persona is designed with visual identity, voice, and personality traits
- [ ] **BRAND-02**: Character appears consistently in all videos (avatar, intro sequence, visual treatment)
- [ ] **BRAND-03**: Yocoya.ai branding (logo, URL) is visible in every video

### Human Review

- [ ] **REVEW-01**: Generated videos queue in Airtable with status tracking (generated, reviewing, approved, rejected, posted)
- [ ] **REVEW-02**: Reviewer receives notification when new video is ready for review
- [ ] **REVEW-03**: Approved videos automatically trigger the posting workflow
- [ ] **REVEW-04**: Rejected videos can be flagged with notes for regeneration

### Distribution

- [ ] **DISTR-01**: Approved videos auto-post to TikTok via Blotato
- [ ] **DISTR-02**: Approved videos auto-post to Instagram Reels via Blotato
- [ ] **DISTR-03**: Approved videos auto-post to YouTube Shorts via Blotato
- [ ] **DISTR-04**: Approved videos auto-post to X/Twitter via Blotato
- [ ] **DISTR-05**: Each post includes platform-optimized caption, hashtags, and CTA link to yocoya.ai landing page
- [ ] **DISTR-06**: Posts are scheduled at optimal times per platform (not all simultaneous)

### Content Sourcing

- [ ] **SOURC-01**: Pipeline can pull tool data from Webflow CMS Herramientas collection to auto-generate spotlight video topics
- [ ] **SOURC-02**: Pipeline monitors AI news feeds (existing Airtable/RSS pipeline) to detect trending topics for explainer videos
- [ ] **SOURC-03**: Content mix targets 70% AI term explainers, 30% tool spotlights
- [ ] **SOURC-04**: Pipeline produces 3-5 videos per week

### Tracking & Analytics

- [ ] **TRACK-01**: Each video has a unique tracking URL (UTM params) linking to a specific yocoya.ai landing page
- [ ] **TRACK-02**: UTM params include source (platform), medium (video), campaign (topic/tool slug)
- [ ] **TRACK-03**: Platform analytics (views, likes, shares) are logged in Airtable per video
- [ ] **TRACK-04**: Auto-generated hashtags and descriptions are optimized per platform for discoverability

### Error Handling

- [ ] **ERROR-01**: Each pipeline step logs success/failure in n8n execution log
- [ ] **ERROR-02**: Critical failures (API errors, rendering failures) trigger alert notification
- [ ] **ERROR-03**: Failed steps have retry logic with exponential backoff

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### Optimization

- **OPTIM-01**: A/B test different hooks and CTAs to optimize engagement per platform
- **OPTIM-02**: Content performance prediction model prioritizes high-potential topics before generation
- **OPTIM-03**: Auto-repurpose underperforming videos (new hook, different platform, recut)

### Advanced Visuals

- **ADVVS-01**: AI-generated b-roll and images (Midjourney/DALL-E/Runway) replace stock footage
- **ADVVS-02**: Dynamic scene transitions and motion graphics

### Expansion

- **EXPAN-01**: Portuguese language support for Brazilian market
- **EXPAN-02**: Multiple character personas for different content verticals
- **EXPAN-03**: Long-form video versions (YouTube full, podcast clips)

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| Fully automated publishing (no review) | Brand risk -- AI errors, factual mistakes go live. Human-in-the-loop is non-negotiable. |
| Custom video editing UI | Scope creep -- compete with established tools. Use templates + external tools for rare manual edits. |
| Comment/engagement automation | Platforms detect/ban bots. Damages brand trust. Human engagement only. |
| Real-time video generation | Costs spike, reliability drops. Batch processing sufficient for scheduled content. |
| Support for 10+ platforms | Spreads thin. Nail 4 platforms first, add others only if data proves demand. |
| Case study / walkthrough videos | Deferred to v2 -- focus on content machine first. |
| Direct platform API posting | Using Blotato. Only revisit if Blotato proves unreliable. |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| (populated by roadmapper) | | |

**Coverage:**
- v1 requirements: 34 total
- Mapped to phases: 0
- Unmapped: 34

---
*Requirements defined: 2026-02-08*
*Last updated: 2026-02-08 after initial definition*
