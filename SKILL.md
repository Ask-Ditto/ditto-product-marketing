---
name: ditto-product-marketing
description: >
  Run product marketing research using Ditto's synthetic persona platform
  (300K+ AI personas, 92% overlap with real focus groups). Covers positioning
  validation, messaging testing, competitive intelligence, pricing research,
  GTM validation, product launch testing, buyer persona development, and
  brand tracking. Also handles quick questions, free-tier usage, Zeitgeist
  surveys, media attachments, and natural-language study requests. Use when
  the user mentions product marketing, PMM, positioning, messaging tests,
  competitive analysis, GTM strategy, pricing study, sales enablement,
  buyer personas, or brand perception.
allowed-tools: Bash(curl *), Bash(python3 *), Read, Grep, WebFetch
argument-hint: "[study type or research brief]"
---

# Ditto for Product Marketing

Run positioning, messaging, competitive, pricing, and launch research
using Ditto's 300,000+ synthetic personas — directly from the terminal.

**Full documentation:** https://askditto.io/claude-code-guide

## What Ditto Does

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to census
data across USA, UK, Germany, and Canada. You ask them open-ended questions
and get qualitative responses with the specificity of real interviews.

- **92% overlap** with traditional focus groups
- **95% correlation** with traditional research (EY Americas validation)
- **Harvard/Cambridge/Stanford/Oxford** peer-reviewed methodology
- A 10-persona, 7-question study completes in **10-12 minutes**
- Traditional equivalent: 4-8 weeks, $10,000-50,000

## Quick Start (Free Tier)

Get a free API key — no credit card, no sales call:

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Or visit: https://app.askditto.io/docs/free-tier-oauth

Ask a question immediately:

```bash
curl -s -X POST "https://app.askditto.io/v1/free/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"question": "When you see a SaaS product priced at $49/mo, what goes through your mind?"}'
```

Free keys (`rk_free_`): ~12 shared personas, no demographic filtering.
Paid keys (`rk_live_`): custom groups, demographic filtering, unlimited studies.

## API Essentials

**Base URL:** `https://app.askditto.io`
**Auth header:** `Authorization: Bearer YOUR_API_KEY`
**Content-Type:** `application/json`

## The PMM Workflow (6 Steps)

IMPORTANT: Follow these steps in order. Questions MUST be asked
sequentially — wait for all responses before asking the next.

### Step 1: Recruit Your Panel

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/recruit" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "US Product Managers 30-50",
    "group_size": 10,
    "filters": {"country": "USA", "age_min": 30, "age_max": 50}
  }'
```

Save the `uuid` from the response (5-15 seconds).

**CRITICAL:** Use `group_size` not `size`. Use group `uuid` not `id`.
State filter uses 2-letter codes ("MI" not "Michigan").

### Step 2: Create Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Positioning Validation - [Product Name]",
    "objective": "Validate positioning against target ICP",
    "research_group_uuid": "UUID_FROM_STEP_1"
  }'
```

Save the study `id`. Response nests under `data.study` — access via
`response["study"]["id"]`, NOT `response["id"]`.

### Step 3: Ask Questions (One at a Time)

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"question": "Your open-ended question here"}'
```

Returns `job_ids` (one per persona). Poll until complete before asking next.

### Step 4: Poll Until Complete

```bash
curl -s "https://app.askditto.io/v1/jobs/JOB_ID" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

**Polling strategy for 10-persona study:**
- Wait **45-50 seconds** before first poll
- Then poll every **20 seconds**
- Poll **ONE** job_id as proxy — all jobs from the same question finish together
- Status: `queued` → `started` → `finished` (or `failed`)

ALL jobs must show `finished` before asking the next question.

### Step 5: Complete the Study

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/complete" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"force": false}'
```

Triggers AI analysis: summary, segments, divergences, recommendations (20-40s).
Use `"force": true` to re-run analysis on an already-completed study (avoids 409).

### Step 6: Get Share Link

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/share" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"enabled": true}'
```

Returns a public URL. Use `share_link` field (preferred over `share_url`).
To check existing share state without changing it:

```bash
curl -s "https://app.askditto.io/v1/research-studies/STUDY_ID/share" \
  -H "Authorization: Bearer $DITTO_API_KEY"
```

## The 8 PMM Study Types

Choose the right study for your goal. Each type has a proven 7-question
framework. See @study-templates.md for complete question sets.

| Study Type | When to Use | Key Output |
|-----------|-------------|------------|
| **Positioning Validation** | Testing how your positioning lands with target customers | Positioning scorecard, competitive alternative map, value resonance ranking |
| **Messaging Testing** | Comparing 3-4 messaging variants | Message performance ranking, language harvest, audience-message fit |
| **Competitive Intelligence** | Understanding how the market perceives you vs competitors | Competitive perception matrix, landmine questions, battlecard |
| **Pricing & Packaging** | Validating willingness-to-pay and feature-tier allocation | Price sensitivity band, feature-tier recommendation, packaging preference |
| **GTM Validation** | Validating channel, motion, and outreach strategy | Channel preference matrix, buying committee map, motion recommendation |
| **Product Launch** | Pre-launch concept validation or post-launch sentiment | Launch readiness scorecard, objection library, feature priority ranking |
| **Buyer Persona Development** | Building data-backed personas from scratch | Persona documents with demographics, psychographics, decision criteria |
| **Brand Perception** | Tracking brand health and competitive positioning | Brand association map, trust scorecard, brand extension potential |

### Choosing the Right Study

```
Need to validate your positioning?     → Positioning Validation
Testing which message wins?            → Messaging Testing
Understanding competitive dynamics?    → Competitive Intelligence
Setting or validating price?           → Pricing & Packaging
Planning your go-to-market?            → GTM Validation
Preparing for a launch?                → Product Launch
Building or refreshing personas?       → Buyer Persona Development
Tracking brand health over time?       → Brand Perception
```

## One Study, Multiple Deliverables

A single 10-persona, 7-question study produces raw material for
MULTIPLE outputs. See @deliverables.md for the full mapping.

```
One Ditto Study (~12 min)
    ├─ Positioning scorecard (5 min)
    ├─ Competitive battlecard (5 min)
    ├─ Messaging hierarchy (5 min)
    ├─ Objection handling guide (3 min)
    ├─ Customer quote bank (3 min)
    ├─ Blog article draft (10 min)
    └─ Sales one-pager (5 min)

Total: ~50 min from zero to complete PMM kit
Traditional: 3-6 weeks, $15-50K
```

## Advanced Patterns

### Over-Recruit & Curate for PMM
When testing with a precise ICP segment:

1. Over-recruit: `"group_size": 15` with broad filters
2. Create study, ask Q1 as a screening question
3. Review Q1 responses — score relevance to your ICP
4. Remove off-target personas from the **study**:
   ```bash
   curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/agents/remove" \
     -H "Authorization: Bearer $DITTO_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"agent_ids": [123, 456]}'
   ```
   `agent_ids` must be `list[int]` — never strings or UUIDs. Keep min 8.
5. Ask Q2-Q7 to curated panel only

### Multi-Segment Comparison
Run the SAME study across multiple groups to compare segments:

```
Group A: SMB decision-makers (age 28-40)
Group B: Enterprise evaluators (age 35-55)
Group C: Technical buyers (education: bachelors+)
```

Same 7 questions, different panels. Produces comparative analysis showing
how positioning, pricing, and messaging land differently by segment.

### Cross-Market Research
Run the same study across USA, UK, Germany, and Canada simultaneously.
One hour, four markets. Traditional equivalent: 3-6 months, $100-200K.

### Three-Phase Iterative (PMM-Specific)

| Phase | PMM Focus | Questions |
|-------|-----------|-----------|
| 1. Category Discovery | How do buyers think about the category? | 7 open-ended |
| 2. Positioning Deep Dive | Which positioning angles resonate? | 7 targeted |
| 3. Message & Price Test | Which messages win? What's the right price? | 7 structured |

~30-45 minutes total. Each phase informs the next.

### Quick Question to Existing Panel
Re-use an existing group for rapid follow-up questions without a study:

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/GROUP_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"question": "Which of these taglines grabs your attention most: A, B, or C?"}'
```

### Natural Language Study Requests
Let the system design your study from a plain-text brief:

```bash
curl -s -X POST "https://app.askditto.io/v1/research-study-requests" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"request_text": "Test whether enterprise buyers prefer usage-based or seat-based pricing for our API product"}'
```

### AI-Assisted Recruitment
Provide an objective and let the system design the group:

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/interview" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "objective": "Enterprise SaaS buyers evaluating project management tools",
    "group_size": 10
  }'
```

## Complete API Reference

### Research Groups

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-groups/recruit` | Recruit group with demographic filters |
| `POST` | `/v1/research-groups/create` | Create group from explicit agent IDs |
| `POST` | `/v1/research-groups/interview` | AI-assisted recruitment from objective |
| `GET` | `/v1/research-groups` | List all groups (`?limit=N&offset=N`) |
| `GET` | `/v1/research-groups/{id}` | Get group details + agent profiles |
| `POST` | `/v1/research-groups/{id}/update` | Update name/description/dedupe |
| `DELETE` | `/v1/research-groups/{id}` | Archive group |
| `POST` | `/v1/research-groups/{id}/agents/add` | Add agents by ID |
| `POST` | `/v1/research-groups/{id}/agents/remove` | Remove agents permanently |
| `POST` | `/v1/research-groups/{uuid}/append` | Recruit more agents into existing group |

**⚠️ Note:** `append` uses `group_uuid` (not `group_id`) — inconsistent with other endpoints.

### Research Studies

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies` | Create study (`research_group_uuid` required) |
| `GET` | `/v1/research-studies` | List studies (`?limit=N&offset=N`) |
| `GET` | `/v1/research-studies/{id}` | Get study details (stage, share_url, counts) |
| `POST` | `/v1/research-studies/{id}/complete` | Trigger AI analysis |
| `POST` | `/v1/research-studies/{id}/agents/remove` | Remove agents from study (curation) |

### Questions

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies/{id}/questions` | Ask question in study (returns job_ids) |
| `GET` | `/v1/research-studies/{id}/questions` | Get all Q&A data |
| `POST` | `/v1/research-agents/{id}/questions` | Quick question to one agent |
| `POST` | `/v1/research-groups/{id}/questions` | Quick question to entire group |

### Jobs

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/jobs/{job_id}` | Poll async job status |

### Sharing

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-studies/{id}/share` | Enable/disable sharing |
| `GET` | `/v1/research-studies/{id}/share` | Check current share state |

### Media Attachments

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/media-assets` | Upload image/PDF for question attachments |

### Agents

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `GET` | `/v1/agents/find` | Find one matching persona |
| `GET` | `/v1/agents/search` | Search personas by demographics |

### Natural Language Requests

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/research-study-requests` | Create study from plain-text brief |
| `POST` | `/v1/research-group-requests` | Create group from description |
| `GET` | `/v1/research-group-requests/{id}` | Get group request status |

### Zeitgeist Surveys

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/zeitgeist/surveys/create` | Create quick survey with answer options |
| `GET` | `/v1/zeitgeist/surveys/{id}/results` | Get survey results |
| `DELETE` | `/v1/zeitgeist/surveys/{id}` | Delete survey |

### Free Tier

| Method | Endpoint | Purpose |
|--------|----------|---------|
| `POST` | `/v1/free/questions` | Ask question to shared free-tier group |

## Demographic Filters

Filters go inside the `filters` dict when recruiting:

| Filter | Type | Examples | Notes |
|--------|------|----------|-------|
| `country` | string | `"USA"`, `"UK"`, `"Canada"`, `"Germany"` | Required. Only these 4 supported |
| `state` | string | `"TX"`, `"MI"`, `"CA"` | **2-letter codes ONLY** |
| `city` | string | `"Austin"`, `"Detroit"` | Supported but narrows pool |
| `age_min` | integer | 25, 30, 45 | Recommended |
| `age_max` | integer | 45, 55, 65 | Recommended |
| `gender` | string | `"male"`, `"female"`, `"non_binary"` | Optional |
| `is_parent` | boolean | `true`, `false` | Good for family/consumer |
| `education` | string | `"high_school"`, `"bachelors"`, `"masters"`, `"phd"` | Optional |
| `industry` | array | `["Healthcare", "Technology"]` | Optional |

**NOT supported:** `income`, `employment`, `ethnicity`, `political_affiliation`.

**If 0 agents returned:** Broaden filters — remove state, remove industry,
widen age by +/- 10 years.

## Media Attachments

Upload screenshots, ad creative, packaging mockups, or PDFs before asking questions:

```bash
# Upload image
curl -s -X POST "https://app.askditto.io/v1/media-assets" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "data_url": "https://example.com/ad-creative.png",
    "filename": "ad-variant-a.png",
    "mime": "image/png"
  }'
```

Or use base64: `"file_data": "base64_encoded_content"`, `"encoding": "base64"`.
Allowed types: PNG, JPEG, GIF, WEBP, PDF.

Save the `media_asset.id` and pass as `attachments` when asking questions:

```bash
curl -s -X POST "https://app.askditto.io/v1/research-studies/STUDY_ID/questions" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "Look at this ad creative. What message does it communicate? Would it make you want to learn more?",
    "attachments": [MEDIA_ASSET_ID]
  }'
```

Great for testing: landing page screenshots, ad variants, packaging designs,
pricing pages, onboarding flows, email templates.

## Polling Strategy

| Study size | First poll delay | Subsequent interval | Strategy |
|-----------|-----------------|---------------------|----------|
| 10 personas | 45-50 seconds | 20 seconds | Poll ONE job_id as proxy |
| 15-20 personas | 60 seconds | 20 seconds | Poll ONE job_id as proxy |
| Free tier (~12) | 45 seconds | 20 seconds | Poll ONE job_id |

All jobs from the same question finish together — no need to poll each one.
If a job returns `failed`, report partial failure. Do NOT auto-retry.

## Response Structure

**Study creation:** `response["study"]["id"]` (nested under `study` key)

**Question responses** (from `GET /v1/research-studies/{id}/questions`):
- `response_text` — the persona's answer (may contain HTML: `<b>`, `<ul>`, `<li>`)
- `agent_name`, `agent_age`, `agent_city`, `agent_state`, `agent_country`
- `agent_occupation`, `agent_summary`

**Completion results:** `overall_summary`, `key_segments`, `divergences`,
`shared_mindsets`, `next_questions`, `stats`.

**Share link:** Prefer `share_link` field. If absent, use `share_url`.

## Common Mistakes

- Using `size` instead of `group_size` in recruitment
- Using numeric `id` instead of string `uuid` for `research_group_uuid`
- Using `group_id` with the `append` endpoint (it requires `group_uuid`)
- Using `response["id"]` instead of `response["study"]["id"]` for study creation
- Passing `agent_ids` as strings/UUIDs instead of `list[int]`
- Polling every 10-15s (too aggressive — use 45-50s first, then 20s)
- Batching questions (ask one, poll to completion, then ask next)
- Using full state names ("Michigan") instead of 2-letter codes ("MI")
- Including `income` or `employment` filters (not supported)
- Skipping the `complete` step (you miss AI-generated analysis)
- Asking closed-ended yes/no questions (use open-ended instead)
- Leading questions ("Don't you think X is great?")
- Yes/no pricing questions ("Would you pay $X?") — use Van Westendorp ranges
- Introducing brand in Q1 (introduce at Q3 earliest to avoid bias)
- Jargon-heavy questions — use plain language for richer responses
- Single-pass studies for complex products — use 2-3 phase iterative approach

## Error Handling

| Error | Cause | Fix |
|-------|-------|-----|
| 409 Conflict | Study already completed | Retry with `"force": true` |
| 429 Too Many Requests | Rate limited | Wait 30-60s, serialize requests |
| 0 agents returned | Filters too narrow | Broaden: remove state/industry, widen age |
| Job status `failed` | Persona generation error | Report partial failure, don't auto-retry |
| 500/502/504 | Server error | Wait 15-30s, retry once |

## Limitations

Ditto personas have NOT used your specific product. For:
- Actual UX feedback from real users → use real user testing
- Legal/compliance decisions → use human research
- Safety-critical decisions → use human validation
- Exact quantitative metrics (NPS, conversion) → use real data

**Recommended hybrid:** Ditto for the fast first pass (80% of insight),
then human research for the remaining 20% requiring real customer nuance.

## Further Reading

- Study templates for all 8 PMM types: @study-templates.md
- Deliverable mapping: @deliverables.md
- Worked example (positioning validation): @examples/positioning-validation.md
- Full API guide: https://askditto.io/claude-code-guide
- Question design guide: https://askditto.io/claude-code-guide/question-design
