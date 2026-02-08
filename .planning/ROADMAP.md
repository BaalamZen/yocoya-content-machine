# Roadmap: Yocoya Content Machine

## Overview

The journey from idea to production-ready automated video pipeline: validate critical dependencies (Spanish TTS quality, Blotato API), build core automation (script generation → voice → video assembly), add Professor Glitch branding and human review gates, enable multi-platform distribution, automate content sourcing from existing data, instrument analytics tracking, and harden error handling. Each phase delivers a verifiable capability, building toward a 3-5 videos/week cadence driving traffic to yocoya.ai landing pages.

## Phases

**Phase Numbering:**
- Integer phases (0-9): Planned milestone work
- Decimal phases (X.1, X.2): Urgent insertions (marked with INSERTED)

Decimal phases appear between their surrounding integers in numeric order.

- [ ] **Phase 0: Tech Validation** - Verify Blotato API and Spanish TTS quality before architecture commitment
- [ ] **Phase 1: Script Generation** - n8n workflow generates structured video scripts from topic inputs
- [ ] **Phase 2: Audio Pipeline** - Spanish TTS voiceover generation with consistent character voice
- [ ] **Phase 3: Video Assembly** - Combine audio, visuals, and subtitles into platform-ready videos
- [ ] **Phase 4: Character & Brand** - Professor Glitch persona with consistent visual identity
- [ ] **Phase 5: Review Queue** - Human approval gate with Airtable workflow integration
- [ ] **Phase 6: Distribution** - Multi-platform posting via Blotato with platform-specific optimization
- [ ] **Phase 7: Content Sourcing** - Automated topic selection from Webflow CMS and news feeds
- [ ] **Phase 8: Analytics & Tracking** - UTM tracking and performance metrics per video
- [ ] **Phase 9: Error Handling** - Logging, alerts, and retry logic for production reliability

## Phase Details

### Phase 0: Tech Validation
**Goal**: Confirm Blotato API works for all 4 platforms and ElevenLabs es-MX voice quality meets native speaker standards
**Depends on**: Nothing (first phase)
**Requirements**: VALID-01, VALID-02, VALID-03
**Success Criteria** (what must be TRUE):
  1. Blotato API successfully posts a test video to TikTok, Instagram Reels, YouTube Shorts, and X/Twitter with captions and links preserved
  2. ElevenLabs Spanish (es-MX) voice generates a 60-second audio sample approved by native Mexican speaker for pronunciation, intonation, and naturalness
  3. HeyGen or alternative service can create a custom avatar character (Professor Glitch design validated, API or manual upload process documented)
**Plans**: TBD

Plans:
- [ ] 00-01: TBD

### Phase 1: Script Generation
**Goal**: n8n workflow generates structured video scripts (hook, body, CTA) from topic inputs using LLM
**Depends on**: Phase 0
**Requirements**: SCRPT-01, SCRPT-02, SCRPT-03, SCRPT-04
**Success Criteria** (what must be TRUE):
  1. User provides topic input, n8n workflow generates a 30-60 second video script with hook, body, and CTA sections
  2. AI term explainer template produces scripts for concepts like "LLMs", "RAG", "Generative AI" with consistent educational format
  3. Tool spotlight template pulls data from Webflow CMS Herramientas collection and generates spotlight scripts including name, features, pricing, and affiliate link
  4. All scripts include platform-specific CTA directing viewers to matching yocoya.ai landing page URL
**Plans**: TBD

Plans:
- [ ] 01-01: TBD

### Phase 2: Audio Pipeline
**Goal**: Text-to-speech generates production-quality Spanish (es-MX) voiceover with consistent character voice
**Depends on**: Phase 1
**Requirements**: VOICE-01, VOICE-02, VOICE-03
**Success Criteria** (what must be TRUE):
  1. n8n workflow takes script text and generates Spanish (es-MX) audio file using ElevenLabs or validated affiliate alternative
  2. Same named voice is used across all videos for Professor Glitch character persona recognition
  3. Audio quality is production-ready with no artifacts, natural pacing, and correct pronunciation of AI technical terms
**Plans**: TBD

Plans:
- [ ] 02-01: TBD

### Phase 3: Video Assembly
**Goal**: Pipeline combines audio, visuals, and subtitles into platform-ready vertical video files
**Depends on**: Phase 2
**Requirements**: VIDEO-01, VIDEO-02, VIDEO-03, VIDEO-04, VIDEO-05, VIDEO-06, VISUL-01
**Success Criteria** (what must be TRUE):
  1. n8n workflow outputs final video file in 9:16 vertical format, 30-60 seconds duration, MP4 codec
  2. Spanish subtitles are auto-generated and synced to voiceover timing
  3. English subtitles are generated as secondary track or overlay
  4. Videos meet platform specifications for all 4 targets (TikTok, Instagram, YouTube, X) including resolution, file size, and safe zones for text overlays
  5. Visual assets (stock footage, images, or AI-generated) are mapped to script sections and assembled with audio
  6. Background music bed from Artlist library is mixed under voiceover at appropriate volume level
  7. Transition and glitch SFX from Artlist library are applied at scene changes
**Plans**: TBD

Plans:
- [ ] 03-01: TBD

### Phase 4: Character & Brand
**Goal**: Professor Glitch persona appears consistently in all videos with yocoya.ai branding
**Depends on**: Phase 3
**Requirements**: BRAND-01, BRAND-02, BRAND-03, BRAND-04, VISUL-02, VISUL-03
**Success Criteria** (what must be TRUE):
  1. Professor Glitch character is designed with defined visual identity, voice persona, and personality traits documented
  2. Character avatar appears in every video with consistent intro sequence and visual treatment
  3. Yocoya.ai logo and URL are visible in all videos (intro/outro or persistent overlay)
  4. Visual style matches Professor Glitch aesthetic reference (retro/glitch theme, animated text overlays, rapid cuts)
  5. Brand consistency is maintained across all videos (color palette, typography, transition style)
  6. Curated Artlist audio library maintained in cloud storage (10-15 music beds, 20-30 SFX) categorized by video type and mood
**Plans**: TBD

Plans:
- [ ] 04-01: TBD

### Phase 5: Review Queue
**Goal**: Human-in-the-loop review and approval gate before any video posts to social platforms
**Depends on**: Phase 4
**Requirements**: REVEW-01, REVEW-02, REVEW-03, REVEW-04
**Success Criteria** (what must be TRUE):
  1. Generated videos are added to Airtable review queue with status tracking (generated, reviewing, approved, rejected, posted)
  2. Reviewer receives notification (email, Slack, or other mechanism) when new video is ready for review
  3. Approved videos automatically trigger the posting workflow via n8n webhook or polling
  4. Rejected videos can be flagged with notes for regeneration, blocking them from posting workflow
**Plans**: TBD

Plans:
- [ ] 05-01: TBD

### Phase 6: Distribution
**Goal**: Approved videos auto-post to all 4 platforms with platform-specific optimization
**Depends on**: Phase 5
**Requirements**: DISTR-01, DISTR-02, DISTR-03, DISTR-04, DISTR-05, DISTR-06
**Success Criteria** (what must be TRUE):
  1. Approved video posts to TikTok via Blotato with platform-optimized caption, hashtags, and CTA link
  2. Approved video posts to Instagram Reels via Blotato with platform-optimized caption, hashtags, and CTA link
  3. Approved video posts to YouTube Shorts via Blotato with platform-optimized caption, hashtags, and CTA link
  4. Approved video posts to X/Twitter via Blotato with platform-optimized caption, hashtags, and CTA link
  5. Each platform post includes specific caption, hashtags, and tracking URL tailored to that platform's best practices
  6. Posts are scheduled at optimal times per platform (staggered, not simultaneous) based on audience analytics
**Plans**: TBD

Plans:
- [ ] 06-01: TBD

### Phase 7: Content Sourcing
**Goal**: Automated topic selection from Webflow CMS and news feeds produces 3-5 video topics per week
**Depends on**: Phase 6
**Requirements**: SOURC-01, SOURC-02, SOURC-03, SOURC-04, SOURC-05, SOURC-06
**Success Criteria** (what must be TRUE):
  1. Pipeline queries Webflow CMS Herramientas collection, filtering for published tools with complete data (description, features, pricing, affiliate link)
  2. Pipeline checks Airtable content log to skip tools that already have videos, avoiding duplicates
  3. Pipeline prioritizes tools with active affiliate links for spotlight videos (revenue-first ordering)
  4. Pipeline monitors existing Airtable/RSS news feeds to detect trending AI topics for explainer videos
  5. Content mix automatically targets 70% AI term explainers, 30% tool spotlights
  6. Pipeline produces 3-5 video topics per week without manual topic input
**Plans**: TBD

Plans:
- [ ] 07-01: TBD

### Phase 8: Analytics & Tracking
**Goal**: Every video has unique tracking URL and performance metrics logged for optimization
**Depends on**: Phase 7
**Requirements**: TRACK-01, TRACK-02, TRACK-03, TRACK-04
**Success Criteria** (what must be TRUE):
  1. Each video has unique UTM-tagged tracking URL linking to specific yocoya.ai landing page
  2. UTM parameters include source (platform), medium (video), campaign (topic/tool slug) for attribution tracking
  3. Platform analytics (views, likes, shares, comments) are logged in Airtable per video after posting
  4. Hashtags and post descriptions are auto-generated and optimized per platform for discoverability
**Plans**: TBD

Plans:
- [ ] 08-01: TBD

### Phase 9: Error Handling
**Goal**: Production-ready reliability with logging, alerts, and retry logic for all critical steps
**Depends on**: Phase 8
**Requirements**: ERROR-01, ERROR-02, ERROR-03
**Success Criteria** (what must be TRUE):
  1. Each n8n workflow step logs success/failure to execution log with timestamped entries
  2. Critical failures (API errors, rendering failures, posting failures) trigger alert notification (email, Slack, or webhook)
  3. Failed steps have retry logic with exponential backoff (3 retries with 2x delay increase per retry)
**Plans**: TBD

Plans:
- [ ] 09-01: TBD

## Progress

**Execution Order:**
Phases execute in numeric order: 0 → 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 0. Tech Validation | 0/TBD | Not started | - |
| 1. Script Generation | 0/TBD | Not started | - |
| 2. Audio Pipeline | 0/TBD | Not started | - |
| 3. Video Assembly | 0/TBD | Not started | - |
| 4. Character & Brand | 0/TBD | Not started | - |
| 5. Review Queue | 0/TBD | Not started | - |
| 6. Distribution | 0/TBD | Not started | - |
| 7. Content Sourcing | 0/TBD | Not started | - |
| 8. Analytics & Tracking | 0/TBD | Not started | - |
| 9. Error Handling | 0/TBD | Not started | - |
