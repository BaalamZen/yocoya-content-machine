# Architecture Research

**Domain:** Automated Faceless Short-Form Video Content Pipeline
**Researched:** 2026-02-08
**Confidence:** MEDIUM

## Standard Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                     ORCHESTRATION LAYER (n8n)                        │
├─────────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐            │
│  │ Trigger  │→│ Generate │→│ Assemble │→│  Queue   │→ [Human]     │
│  │ Schedule │  │ Content  │  │ Video    │  │  Review  │             │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘             │
│                      ↓              ↓             ↓                  │
├──────────────────────┴──────────────┴─────────────┴──────────────────┤
│                        PROCESSING LAYER                               │
├─────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                  │
│  │ AI Services │  │ AI Services │  │ AI Services │                  │
│  │ (LLM APIs)  │  │ (Voice Gen) │  │ (Video Gen) │                  │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘                  │
│         │                │                │                          │
├─────────┴────────────────┴────────────────┴──────────────────────────┤
│                        INTEGRATION LAYER                              │
├─────────────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐            │
│  │ Webflow  │  │ Airtable │  │ Blotato  │  │  File    │            │
│  │   CMS    │  │  Tracker │  │ Multi-   │  │ Storage  │            │
│  │          │  │          │  │ Platform │  │  (S3)    │            │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘            │
└─────────────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| **Orchestration Engine** | Workflow coordination, scheduling, error handling, retry logic | n8n (Docker container, port 5678) |
| **Content Selector** | Topic selection from Webflow CMS tool pages, relevance scoring | n8n workflow with Webflow API integration |
| **Script Generator** | Generate video scripts from topics, format for 30-60s videos | Claude/GPT API with custom prompts |
| **Voice Synthesizer** | Text-to-speech in Spanish with character persona | ElevenLabs/LOVO/Murf.ai voice cloning APIs |
| **Visual Asset Generator** | Create images, animations, B-roll for video scenes | Diffus/OpenArt/VistaCreate image APIs |
| **Video Assembler** | Combine voice, visuals, subtitles into final video | InVideo/HeyGen/Zebracat video composition APIs |
| **Subtitle Generator** | Generate English subtitles from Spanish audio | Whisper API or built-in video platform features |
| **Review Queue** | Human approval before publishing | Airtable with status tracking + n8n webhook |
| **Multi-Platform Publisher** | Post to TikTok, Reels, Shorts, X with platform-specific formatting | Blotato API integration |
| **Analytics Tracker** | Monitor video performance, landing page conversions | Airtable logging + platform APIs |
| **State Store** | Track pipeline progress, asset versions, retry state | Airtable (yocoya_publish_queue, yocoya_ingest_log) |
| **Asset Storage** | Persist generated audio, video, image files | S3-compatible storage or file system mount |

## Recommended Pipeline Structure

```
workflows/
├── main-content-pipeline.json       # Primary orchestration workflow
│   ├── trigger/                     # Scheduled topic selection
│   ├── generate/                    # Content generation stage
│   ├── assemble/                    # Video assembly stage
│   ├── review/                      # Human review queue
│   └── publish/                     # Multi-platform posting
├── subtask-script-generation.json   # Isolated script generation workflow
├── subtask-voice-generation.json    # Isolated voice synthesis workflow
├── subtask-video-assembly.json      # Isolated video composition workflow
└── subtask-platform-posting.json    # Isolated multi-platform posting workflow

assets/
├── audio/                           # Generated voiceovers
│   └── {video-id}/                  # One folder per video
│       ├── raw.mp3                  # Original Spanish audio
│       └── normalized.mp3           # Processed audio for video
├── images/                          # Generated visual assets
│   └── {video-id}/
│       ├── scene-01.png
│       ├── scene-02.png
│       └── thumbnail.png
├── videos/                          # Assembled videos
│   └── {video-id}/
│       ├── draft.mp4                # Pre-review version
│       ├── final.mp4                # Approved version
│       └── platform-variants/       # Platform-specific encodes
│           ├── tiktok.mp4           # 9:16, max 60s
│           ├── reels.mp4            # 9:16, max 90s
│           ├── shorts.mp4           # 9:16, max 60s
│           └── twitter.mp4          # 16:9 or 9:16, max 140s
└── subtitles/                       # Generated subtitle files
    └── {video-id}/
        ├── en.srt                   # English subtitles
        └── es.srt                   # Spanish subtitles (optional)

state/
└── airtable-tables/                 # External state in Airtable
    ├── content-queue               # Topics selected for videos
    ├── publish-queue               # Videos awaiting review/publish
    ├── ingest-log                  # Historical processing records
    └── analytics                   # Performance tracking
```

### Structure Rationale

- **workflows/:** Modular n8n workflows for isolation and reusability. Main pipeline orchestrates subtask workflows via Execute Workflow nodes. Allows parallel processing and independent retries.
- **assets/:** Organized by video-id for clear lifecycle management. Each video is a self-contained unit with all generated assets. Enables easy cleanup and archival.
- **state/:** Centralized in Airtable for human visibility and manual intervention capability. n8n is ephemeral; Airtable is source of truth for pipeline state.

## Architectural Patterns

### Pattern 1: Pipeline with Human-in-the-Loop

**What:** Automated stages with mandatory human review before irreversible actions (publishing).

**When to use:** When content quality/brand safety requires human judgment but 90% of work can be automated.

**Trade-offs:**
- Pros: Prevents AI hallucination disasters, maintains brand quality, enables continuous improvement
- Cons: Introduces latency, requires human availability, creates queue management complexity

**Example:**
```javascript
// n8n workflow structure
{
  nodes: [
    { name: 'Generate Video', type: 'executeWorkflow' },
    { name: 'Write to Review Queue', type: 'airtable' },
    { name: 'Wait for Webhook', type: 'webhook' },  // Human approves via Airtable automation
    { name: 'Check Approval Status', type: 'if' },
    { name: 'Publish to Platforms', type: 'executeWorkflow' },
    { name: 'Archive Rejected', type: 'move' }
  ]
}
```

### Pattern 2: Async Job Processing with Polling

**What:** Submit long-running tasks (video generation) to external APIs, poll for completion, handle failures gracefully.

**When to use:** When external API operations take minutes to hours (video rendering, voice synthesis).

**Trade-offs:**
- Pros: Avoids n8n execution timeouts, enables parallel processing, better resource utilization
- Cons: Complexity in state management, requires robust error handling, harder to debug

**Example:**
```javascript
// n8n polling pattern
{
  nodes: [
    { name: 'Submit Video Job', type: 'httpRequest' },  // Returns job_id
    { name: 'Store Job ID', type: 'airtable' },
    { name: 'Wait 30s', type: 'wait' },
    { name: 'Poll Job Status', type: 'httpRequest' },   // GET /jobs/{job_id}
    { name: 'Check Status', type: 'if' },               // If 'completed' → continue, else loop back
    { name: 'Retrieve Result', type: 'httpRequest' },   // GET /jobs/{job_id}/download
    { name: 'Save to Storage', type: 's3' }
  ]
}
```

### Pattern 3: Workflow Decomposition (Subtask Isolation)

**What:** Break monolithic pipeline into independent, reusable sub-workflows. Main workflow orchestrates via Execute Workflow nodes.

**When to use:** When pipeline stages are complex enough to deserve isolated development/testing, or when stages need independent retry logic.

**Trade-offs:**
- Pros: Easier testing, better error isolation, reusable components, parallel development
- Cons: More workflows to manage, cross-workflow debugging complexity, state passing overhead

**Example:**
```javascript
// Main workflow orchestration
{
  nodes: [
    { name: 'Execute Script Gen', type: 'executeWorkflow', workflow: 'script-generation' },
    { name: 'Execute Voice Gen', type: 'executeWorkflow', workflow: 'voice-generation' },
    { name: 'Execute Video Assembly', type: 'executeWorkflow', workflow: 'video-assembly' }
  ]
}

// Each subtask workflow receives inputs via $json payload and returns outputs
```

### Pattern 4: Parallel Asset Generation with Join

**What:** Generate multiple assets (images, voice, subtitles) in parallel, synchronize before video assembly.

**When to use:** When multiple independent AI API calls can run simultaneously to reduce wall-clock time.

**Trade-offs:**
- Pros: Faster pipeline execution, better API quota utilization
- Cons: Requires synchronization logic, all branches must succeed or pipeline stalls

**Example:**
```javascript
{
  nodes: [
    { name: 'Split Batch', type: 'splitInBatches' },
    // Parallel branches
    { name: 'Generate Scene 1', type: 'httpRequest', branch: 1 },
    { name: 'Generate Scene 2', type: 'httpRequest', branch: 2 },
    { name: 'Generate Scene 3', type: 'httpRequest', branch: 3 },
    { name: 'Merge Results', type: 'merge' }  // Wait for all branches
  ]
}
```

## Data Flow

### Primary Pipeline Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│ 1. TOPIC SELECTION (Webflow CMS)                                     │
│    - Query Webflow for 47+ tool pages                                │
│    - Score by recency, relevance, existing video coverage            │
│    - Select top N topics (e.g., 5 per week)                          │
│    - Write to Airtable content-queue                                 │
│    Output: [{tool_name, webflow_slug, description, priority}]        │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 2. SCRIPT GENERATION (Claude/GPT API)                                │
│    Input: {tool_name, description}                                   │
│    - Generate 30-60s video script in Spanish                         │
│    - Include hook, 3 key points, CTA to yocoya.ai/{slug}             │
│    - Format: [scene_1, scene_2, scene_3, outro]                      │
│    - Store script in Airtable                                        │
│    Output: {video_id, script_json, scene_count}                      │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 3. VOICEOVER GENERATION (ElevenLabs/LOVO/Murf.ai)                    │
│    Input: {script_json}                                              │
│    - Convert script text to Spanish speech                           │
│    - Apply Professor Glitch voice persona                            │
│    - Generate audio file (MP3/WAV)                                   │
│    - Store in assets/audio/{video_id}/                               │
│    Output: {audio_url, duration_seconds}                             │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 4. VISUAL ASSET GENERATION (Diffus/OpenArt/VistaCreate)              │
│    Input: {script_json, scene_count}                                 │
│    - Generate image per scene (parallel API calls)                   │
│    - Prompt: AI-themed visuals matching scene description            │
│    - Store in assets/images/{video_id}/                              │
│    Output: [{scene_id, image_url}]                                   │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 5. VIDEO ASSEMBLY (InVideo/HeyGen/Zebracat)                          │
│    Input: {audio_url, image_urls[], script_json}                     │
│    - Combine audio + images with timing from script                  │
│    - Add Professor Glitch character overlay (faceless persona)       │
│    - Generate English subtitles (auto or Whisper API)                │
│    - Export 9:16 vertical video                                      │
│    - Store in assets/videos/{video_id}/draft.mp4                     │
│    Output: {video_url, duration, subtitle_url}                       │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 6. HUMAN REVIEW QUEUE (Airtable + Webhook)                           │
│    Input: {video_id, video_url, tool_name}                           │
│    - Create Airtable record in yocoya_publish_queue                  │
│    - Status: 'pending_review'                                        │
│    - Notify human reviewer (email/Slack)                             │
│    - Wait for webhook callback from Airtable automation              │
│    - If approved → continue, if rejected → archive                   │
│    Output: {video_id, approved: true/false, feedback}                │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 7. PLATFORM-SPECIFIC ENCODING (FFmpeg or Video API)                  │
│    Input: {video_url}                                                │
│    - Generate platform variants (TikTok, Reels, Shorts, X)           │
│    - TikTok/Shorts: max 60s, 9:16, H.264                             │
│    - Reels: max 90s, 9:16, H.264                                     │
│    - X: max 140s, 9:16 or 16:9, H.264                                │
│    - Store in assets/videos/{video_id}/platform-variants/            │
│    Output: [{platform, video_url, specs}]                            │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 8. MULTI-PLATFORM POSTING (Blotato)                                  │
│    Input: {platform_videos[], caption, landing_page_url}             │
│    - Caption: Spanish text + "Más info: yocoya.ai/{slug}"            │
│    - Post to TikTok, Instagram Reels, YouTube Shorts, X              │
│    - Track post IDs for analytics                                    │
│    - Update Airtable status: 'published'                             │
│    Output: [{platform, post_id, post_url}]                           │
└─────────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────────┐
│ 9. ANALYTICS TRACKING (Airtable + Platform APIs)                     │
│    Input: {post_ids[]}                                               │
│    - Scheduled workflow (daily): fetch views, likes, shares          │
│    - Track landing page traffic from video links                     │
│    - Correlate with affiliate conversions (if applicable)            │
│    - Store in Airtable analytics table                               │
│    Output: [{post_id, views, engagement_rate, conversions}]          │
└─────────────────────────────────────────────────────────────────────┘
```

### State Management Flow

```
Airtable Tables (Source of Truth)
    ↓ (n8n reads)
┌──────────────────────────────┐
│ content-queue                │  ← Topics selected for videos
│ - id                         │
│ - tool_name                  │
│ - webflow_slug               │
│ - status: queued/processing  │
└──────────────────────────────┘
    ↓ (n8n writes)
┌──────────────────────────────┐
│ publish-queue                │  ← Videos in pipeline
│ - video_id                   │
│ - script_json                │
│ - audio_url                  │
│ - video_url                  │
│ - status: generating/review/ │
│   approved/published         │
│ - assigned_reviewer          │
│ - platform_post_ids          │
└──────────────────────────────┘
    ↓ (n8n writes)
┌──────────────────────────────┐
│ ingest-log                   │  ← Historical records
│ - video_id                   │
│ - created_at                 │
│ - published_at               │
│ - error_logs                 │
└──────────────────────────────┘
```

### Retry and Error Handling Flow

```
[API Call Fails]
    ↓
[Check Error Type]
    ├─ Rate Limit → [Wait + Retry with Exponential Backoff]
    ├─ Timeout → [Retry up to 3 times]
    ├─ 5xx Server Error → [Retry with Different API Provider]
    └─ 4xx Client Error → [Log to Airtable + Notify Human + Stop]
```

## Scaling Considerations

| Scale | Architecture Adjustments |
|-------|--------------------------|
| **0-10 videos/week** | Single n8n instance, synchronous processing, Airtable for all state, no caching |
| **10-50 videos/week** | Add parallel asset generation, implement job queue (Bull/Redis), cache Webflow CMS data (1hr TTL), separate review workflow from generation |
| **50+ videos/week** | Horizontal n8n scaling (multiple workers), dedicated job queue (Bull/Redis), S3 for asset storage (not local), CDN for video delivery, separate database for analytics (not Airtable), implement content versioning |

### Scaling Priorities

1. **First bottleneck: AI API rate limits**
   - **Symptoms:** Pipeline stalls waiting for API availability, 429 errors
   - **Fix:** Implement queue with rate limiting (Bull), distribute across multiple API keys, add fallback providers (e.g., ElevenLabs → LOVO → Murf.ai)

2. **Second bottleneck: Video generation time (5-15 minutes per video)**
   - **Symptoms:** Pipeline takes hours for batch processing, n8n execution timeouts
   - **Fix:** Use async job pattern (submit → poll), enable parallel video generation (up to API concurrency limits), separate video assembly into dedicated workflow

3. **Third bottleneck: Human review queue depth**
   - **Symptoms:** Videos pile up awaiting approval, reviewers overwhelmed
   - **Fix:** Implement priority scoring (high-value topics first), batch review UI (Airtable interface), auto-approve low-risk content types after X successful reviews

4. **Fourth bottleneck: Storage costs for video assets**
   - **Symptoms:** Storage bills escalate, slow asset retrieval
   - **Fix:** Implement lifecycle policies (delete drafts after 30 days, archive published videos to Glacier after 90 days), compress assets before storage

## Anti-Patterns

### Anti-Pattern 1: Synchronous Processing in n8n

**What people do:** Chain all API calls synchronously in a single n8n workflow execution (topic → script → voice → images → video → publish).

**Why it's wrong:**
- n8n workflows have execution timeouts (usually 2-10 minutes default)
- Video generation APIs take 5-15 minutes, causing workflow failures
- No ability to retry individual stages without restarting entire pipeline
- One API failure kills the entire batch

**Do this instead:**
- Break pipeline into stage-specific workflows (Execute Workflow nodes)
- Use async job pattern for long-running tasks (submit → store job_id in Airtable → poll in separate workflow)
- Enable "Continue on Fail" for API nodes with explicit error handling

### Anti-Pattern 2: Storing Assets in n8n Binary Data

**What people do:** Pass video/audio files through n8n workflow as binary data items, keeping them in memory.

**Why it's wrong:**
- n8n has memory limits; large files cause crashes
- Binary data doesn't persist between workflow executions
- No asset versioning or rollback capability
- Can't inspect assets outside workflow execution

**Do this instead:**
- Store assets in S3 or file storage immediately after generation
- Pass URLs (not binary data) between n8n nodes
- Use Airtable to track asset URLs and metadata
- Implement asset lifecycle management (retention policies)

### Anti-Pattern 3: Hardcoded API Keys in Workflows

**What people do:** Embed API keys directly in HTTP Request nodes or Function nodes.

**Why it's wrong:**
- Security risk when exporting/sharing workflows
- No key rotation capability without editing workflows
- Can't use different keys per environment (dev/prod)
- Audit trail gaps (can't track which key was used)

**Do this instead:**
- Use n8n Credentials feature for all API keys
- Store sensitive config in environment variables
- Implement key rotation schedule (quarterly)
- Use separate credentials per environment

### Anti-Pattern 4: No Idempotency in Publishing

**What people do:** Retry failed publish operations without checking if post already exists, causing duplicate posts.

**Why it's wrong:**
- Duplicate posts spam followers, damage brand
- Platform rate limits trigger on duplicate content
- Analytics get fragmented across duplicate posts
- No way to correlate posts to original video_id

**Do this instead:**
- Store platform post_ids in Airtable immediately after successful publish
- Check Airtable for existing post_id before retrying publish
- Use platform API "update" endpoints when available (not always possible)
- Implement deduplication logic based on video_id + platform

### Anti-Pattern 5: Monolithic Script Prompts

**What people do:** Use single massive LLM prompt to generate entire script, hoping for consistent JSON structure.

**Why it's wrong:**
- LLMs hallucinate structure (broken JSON parsing)
- No control over scene count or timing
- Hard to iterate on specific scenes without regenerating everything
- Unpredictable video duration (30s vs 90s)

**Do this instead:**
- Multi-stage prompting: hook → key points → scenes → CTA
- Use structured output APIs (GPT-4 with JSON mode, Claude with tool use)
- Validate each stage before proceeding (JSON schema validation)
- Implement duration constraints in prompt ("each scene must be exactly 10 seconds of spoken text")

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| **Webflow CMS** | REST API (OAuth2) | Rate limit: 60 req/min. Cache collection items (1hr TTL). Use `cmsLocaleId` for es-MX/en-US |
| **Airtable** | REST API (Bearer token) | Rate limit: 5 req/sec. Use batch endpoints for bulk operations. Personal access tokens expire |
| **Claude/GPT** | REST API (API key) | Rate limit varies by tier. Implement exponential backoff. Use streaming for long responses |
| **ElevenLabs/LOVO/Murf.ai** | REST API (API key) | Async job pattern required. Polling interval: 5-10s. Download URLs expire (24hr typical) |
| **InVideo/HeyGen/Zebracat** | REST API (API key) | Async job pattern required. Rendering: 5-15 min. Webhook callbacks supported (preferred over polling) |
| **Diffus/OpenArt/VistaCreate** | REST API (API key) | Image generation: 10-30s. Support batch requests. Check NSFW filters (can reject AI content) |
| **Blotato** | REST API (API key) | Multi-platform posting. Rate limits per platform (TikTok: 1/min, IG: 25/day). Returns platform-specific post IDs |
| **S3/CloudFlare R2** | S3-compatible API (AWS SDK) | Use presigned URLs for uploads. Set CORS for browser access. Lifecycle policies for cost control |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| **n8n ↔ Airtable** | REST API (state read/write) | Airtable is source of truth. n8n reads queue, writes status. Use Airtable webhook to trigger n8n (review approval) |
| **n8n ↔ File Storage** | S3 API (asset persistence) | n8n writes assets immediately after generation. Passes URLs (not binary) to subsequent nodes |
| **Main Workflow ↔ Subtask Workflows** | Execute Workflow node (JSON payload) | Parent passes `{video_id, ...inputs}` to child. Child returns `{video_id, ...outputs}`. Timeout: 30min+ for video gen |
| **Generation Stage ↔ Review Stage** | Airtable + Webhook (async handoff) | Generation writes record to publish-queue. Review workflow triggered by Airtable automation webhook |
| **Publishing ↔ Analytics** | Airtable (post_id storage) | Publishing writes `{post_id, platform}` to Airtable. Analytics workflow reads post_ids and fetches platform metrics |

## Bottleneck Analysis

### Pipeline Bottlenecks (by stage)

| Stage | Typical Duration | Bottleneck Type | Mitigation |
|-------|------------------|-----------------|------------|
| **Topic Selection** | 10-30s | API rate limit (Webflow 60/min) | Cache CMS data, batch requests |
| **Script Generation** | 10-30s | LLM API latency | Use streaming, cache prompts, parallel generation |
| **Voice Generation** | 30-90s | API processing time | Async job pattern, parallel scenes |
| **Visual Assets** | 60-180s (3+ images) | API rate limits, generation time | Parallel requests, image caching/reuse |
| **Video Assembly** | 300-900s (5-15min) | **CRITICAL BOTTLENECK** - rendering time | Async job pattern, webhook callbacks, parallel videos |
| **Human Review** | Variable (hours-days) | **CRITICAL BOTTLENECK** - human availability | Priority queue, batch review, auto-approve rules |
| **Platform Encoding** | 30-60s | CPU-bound encoding | Pre-generate variants, use platform-native formats |
| **Multi-Platform Posting** | 10-30s | Platform rate limits | Sequential posting with delays, retry logic |

### Async Processing Requirements

**Must be async (long-running):**
- Voice generation (30-90s)
- Visual asset generation (60-180s total)
- Video assembly (5-15 minutes) ← most critical
- Human review (hours to days)

**Can be sync (< 30s):**
- Topic selection
- Script generation (with streaming)
- Platform encoding (if using external API)
- Multi-platform posting

**Pattern for async stages:**
```javascript
// Submit job
POST /api/video/generate → {job_id: "abc123"}

// Store job_id in Airtable
UPDATE publish-queue SET job_id='abc123', status='rendering'

// Separate polling workflow (triggered every 30s)
GET /api/video/status/abc123 → {status: 'processing', progress: 45}
IF status == 'completed' → download result → update Airtable → trigger next stage
```

## File Storage Strategy

### Storage Architecture

```
Production Storage (S3/CloudFlare R2)
├── videos/
│   └── {video_id}/
│       ├── draft.mp4              # Pre-review (7-day retention)
│       ├── final.mp4              # Approved (90-day hot, then Glacier)
│       └── platforms/
│           ├── tiktok.mp4         # Posted (90-day retention)
│           ├── reels.mp4
│           ├── shorts.mp4
│           └── twitter.mp4
├── audio/
│   └── {video_id}/
│       └── voiceover.mp3          # 30-day retention
└── images/
    └── {video_id}/
        ├── scene-01.png           # 30-day retention
        ├── scene-02.png
        └── thumbnail.png          # 90-day retention (reused in posts)

Temporary Storage (n8n container volume or temp S3 bucket)
└── processing/
    └── {video_id}/                # Deleted after pipeline completion
        ├── temp-audio.wav
        └── temp-renders/
```

### Storage Policies

| Asset Type | Retention | Storage Class | Rationale |
|------------|-----------|---------------|-----------|
| Draft videos | 7 days | Standard | Only needed during review, delete after approval/rejection |
| Final videos | 90 days hot → Glacier | Standard → Glacier | Platforms host final copy, archive for re-posting |
| Platform variants | 90 days | Standard | Platforms host copy, keep for analytics period |
| Audio files | 30 days | Standard | Can regenerate if needed, not needed after publish |
| Images | 30 days | Standard | Can regenerate if needed, not needed after publish |
| Thumbnails | 90 days | Standard | May reuse for social posts |

### File Naming Convention

```
{video_id}_{stage}_{version}_{platform}.{ext}

Examples:
vid-20260208-001_draft_v1.mp4
vid-20260208-001_final_v2.mp4
vid-20260208-001_final_v2_tiktok.mp4
vid-20260208-001_audio_v1.mp3
vid-20260208-001_scene-01_v1.png
```

**video_id format:** `vid-{YYYYMMDD}-{counter}` (e.g., `vid-20260208-001`)
**Airtable primary key:** video_id (consistent across all tables)

## Human Review Queue Design

### Queue Architecture

```
Airtable: yocoya_publish_queue
┌──────────────────────────────────────────────────────────────┐
│ Fields:                                                       │
│ - video_id (primary key)                                     │
│ - tool_name (what video is about)                            │
│ - script_preview (first 100 chars)                           │
│ - video_url (link to draft.mp4 in S3)                        │
│ - thumbnail_url (preview image)                              │
│ - status (pending_review / approved / rejected)              │
│ - assigned_reviewer (single select: Alice, Bob, Charlie)     │
│ - priority (1-5, based on topic relevance)                   │
│ - submitted_at (datetime)                                    │
│ - reviewed_at (datetime)                                     │
│ - feedback (long text, for rejected videos)                  │
│ - revision_count (number)                                    │
│ - platform_post_ids (JSON: {tiktok: "123", reels: "456"})   │
└──────────────────────────────────────────────────────────────┘
                            ↓
              Airtable Automation (webhook)
                            ↓
    When status changes to 'approved' → trigger webhook
                            ↓
               n8n webhook node receives
              {video_id, status: 'approved'}
                            ↓
        n8n continues to publishing stage
```

### Review Workflow (Human Side)

1. Reviewer opens Airtable view filtered by `status = pending_review` and `assigned_reviewer = {me}`
2. Videos sorted by `priority DESC, submitted_at ASC` (high-priority, oldest first)
3. Reviewer clicks `video_url` to watch draft in browser
4. Reviewer checks:
   - Script accuracy (no AI hallucinations)
   - Brand alignment (tone, terminology)
   - Technical quality (audio sync, subtitle timing)
   - CTA correctness (link to right yocoya.ai page)
5. Reviewer updates status:
   - **Approved:** Change status to `approved` → Airtable automation triggers n8n webhook → publishing starts
   - **Rejected:** Change status to `rejected`, add feedback → n8n archives video, optionally triggers regeneration
   - **Needs Revision:** Change status to `needs_revision`, add feedback → n8n triggers revision workflow (regenerate specific scenes)

### Review SLA Targets

| Priority | Target Review Time | Escalation |
|----------|-------------------|------------|
| 1 (High) | 4 hours | Notify via Slack if not reviewed |
| 2-3 (Medium) | 24 hours | Email reminder if not reviewed |
| 4-5 (Low) | 72 hours | No escalation |

### Batch Review Optimization

For reviewers processing 5+ videos:
- Airtable interface with gallery view (thumbnails + play button)
- Keyboard shortcuts via Airtable scripting extension (A = approve, R = reject, N = next)
- Auto-assign round-robin to balance load across reviewers

## Build Order Recommendations

### Phase 1: Core Pipeline (No Human Review)
**Goal:** End-to-end automation for testing, auto-publish to staging accounts

1. Topic selection from Webflow CMS (hardcode 1 topic for testing)
2. Script generation (Claude/GPT API)
3. Voice generation (ElevenLabs with fallback to LOVO)
4. Image generation (Diffus with fallback to OpenArt)
5. Video assembly (InVideo with async job pattern)
6. Storage setup (S3 bucket + lifecycle policies)
7. Airtable state tracking (publish-queue table)

**Validation:** Can generate 1 video end-to-end without manual intervention.

### Phase 2: Review Queue + Manual Publish
**Goal:** Human approval before publishing, manual posting to verify quality

1. Human review queue in Airtable (status field + webhook)
2. n8n webhook receiver for approval events
3. Manual publishing to 1 platform (TikTok via Blotato)
4. Analytics tracking for 1 platform (views, likes)

**Validation:** Human can approve video, manual post succeeds, analytics recorded.

### Phase 3: Multi-Platform Publishing
**Goal:** Automated posting to all platforms after human approval

1. Platform-specific encoding (TikTok, Reels, Shorts, X variants)
2. Blotato integration for 4 platforms
3. Platform-specific caption generation (character limits, hashtags)
4. Post ID tracking in Airtable

**Validation:** Approved video auto-posts to all platforms with correct captions.

### Phase 4: Batch Processing + Scheduling
**Goal:** Generate multiple videos per week on schedule

1. Topic selection algorithm (score 47 tool pages, select top N)
2. Scheduled trigger (e.g., every Monday 9am: select 5 topics)
3. Parallel video generation (up to 3 videos simultaneously)
4. Batch review interface improvements

**Validation:** Weekly batch of 5 videos generates, queues for review, publishes on approval.

### Phase 5: Advanced Features
**Goal:** Optimize quality and efficiency

1. Subtitle generation improvements (English + Spanish)
2. Thumbnail auto-generation (A/B test variants)
3. Content versioning (track script edits, regenerations)
4. Analytics dashboard (aggregate cross-platform metrics)
5. Auto-retry failed API calls with provider fallback

**Validation:** Videos have high-quality subtitles, thumbnails tested, failures auto-recover.

### Dependencies Between Phases

```
Phase 1 (Core Pipeline)
    ↓ (must complete first)
Phase 2 (Review + Manual Publish)
    ↓ (manual publish validates before automation)
Phase 3 (Multi-Platform Automated)
    ↓ (single video flow stable before batch)
Phase 4 (Batch + Scheduling)
    ↓ (optimize after volume increases)
Phase 5 (Advanced Features)
```

**Critical Path:** Phase 1 → Phase 2 must be sequential (no point in review queue without videos to review). Phase 3 can start before Phase 2 is 100% polished (platform encoding can be developed in parallel). Phase 4 requires stable Phase 1-3. Phase 5 is iterative improvements.

## Technology-Specific Notes

### n8n Workflow Patterns

**Execute Workflow Node:**
- Use for subtask isolation (script gen, voice gen, video assembly)
- Set timeout to 30+ minutes for video generation
- Pass video_id as primary identifier through all workflows
- Return outputs as JSON for parent workflow to consume

**HTTP Request Node:**
- Enable "Continue on Fail" for all external API calls
- Use "Retry on Fail" with exponential backoff (3 retries, 2x multiplier)
- Store response in Airtable before proceeding (for retry recovery)

**Wait Node:**
- Use for polling async jobs (30-60s intervals)
- Maximum wait time: 30 minutes (then fail and alert human)

**Webhook Node:**
- Use for human review approval callbacks
- Secure with authentication header (shared secret with Airtable)
- Return immediate 200 OK (don't wait for publishing to complete)

### Airtable as State Store

**Why Airtable:**
- Human-readable state (reviewers can see pipeline progress)
- Built-in UI for review queue (no custom dashboard needed)
- Automation webhooks (trigger n8n on status changes)
- Audit trail (all updates logged with timestamps)

**Limitations:**
- Rate limits (5 req/sec) → use batch endpoints
- No transactions → use status field as lock (optimistic concurrency)
- No complex queries → use n8n for filtering/aggregation

**Alternative for scale (50+ videos/week):**
- PostgreSQL/MongoDB for state (better performance)
- Custom review dashboard (React app)
- Airtable only for human review queue (not full state)

### Video API Provider Selection

**Criteria for choosing video API:**
1. **Async job support:** Must return job_id, not block for 5-15 min
2. **Webhook callbacks:** Preferred over polling (faster, less API calls)
3. **Faceless character support:** Avatar/persona overlay without actual human video
4. **Subtitle auto-generation:** Built-in or easy integration with Whisper API
5. **Platform export presets:** 9:16 vertical, TikTok/Reels-optimized
6. **Pricing model:** Per-minute vs per-video (optimize for 30-60s videos)

**Recommended for this project:**
- **Primary:** Zebracat (faceless AI video, built-in subtitles, webhook support)
- **Fallback:** InVideo (more manual control, async job pattern, cheaper)
- **Avoid:** HeyGen (designed for human avatars, not faceless characters)

## Sources

**Note:** Due to tool access restrictions, this research is based on established patterns from training data (knowledge cutoff: January 2025). Confidence level is MEDIUM as findings could not be verified against current 2026 documentation.

**Architectural patterns drawn from:**
- n8n workflow automation best practices (official docs, community patterns)
- Video content automation systems (InVideo, Zebracat, Pictory architectures)
- Multi-platform social media posting patterns (Buffer, Hootsuite, Later)
- Airtable as workflow state store (common n8n + Airtable integration patterns)
- Async job processing in automation platforms (Bull queue, n8n Execute Workflow)

**Recommendations to validate:**
- Current n8n execution timeout limits (may have changed in 2026)
- Blotato API rate limits per platform (verify before production)
- Video API provider async capabilities (some may have added/removed webhook support)
- S3 lifecycle policy pricing (verify Glacier costs vs deletion)

---
*Architecture research for: Yocoya Content Machine (Automated Faceless Video Pipeline)*
*Researched: 2026-02-08*
*Confidence: MEDIUM (training data, not verified with 2026 sources)*
