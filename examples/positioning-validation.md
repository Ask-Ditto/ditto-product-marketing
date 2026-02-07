# Worked Example: CareQuarter Positioning Validation

A complete 3-phase iterative study validating a startup concept.
Total time: ~4 hours. Total personas: 32. Human intervention: zero.

**Public study links (viewable by anyone):**
- [Phase 1: Pain Discovery](https://app.askditto.io/organization/studies/shared/UlXcv4cjValQu0qJFJrSfL5eKB1zHfc5e2cFZiLYjrA)
- [Phase 2: Deep Dive](https://app.askditto.io/organization/studies/shared/x52Mu1QOwow6fbhornqjY51ug4QI-jY7daoj37ndfAw)
- [Phase 3: Concept Test](https://app.askditto.io/organization/studies/shared/IQiBzKN_q2M3-vSISd_1C7zr2XB66tV6QvVlfy8DkMo)

**Full article:** https://askditto.io/news/ai-founders-use-synthetic-research-to-launch-startup-in-4-hours

---

## Context

CareQuarter: elder care coordination for working adults aged 45-65
managing healthcare for aging parents.

---

## Phase 1: Pain Discovery

**Goal:** Find the real problem (not the assumed one).

**Group:** 12 US adults, age 45-65

```bash
curl -s -X POST "https://app.askditto.io/v1/research-groups/recruit" \
  -H "Authorization: Bearer $DITTO_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "US Adults Managing Elder Care",
    "group_size": 12,
    "filters": {"country": "USA", "age_min": 45, "age_max": 65}
  }'
```

**Questions (7):**
1. Walk me through what healthcare administration looks like for your aging parent. What tasks fall on you?
2. What's the most frustrating part of managing their healthcare? What makes you lose sleep?
3. How much time per week do you spend on healthcare admin for your parent?
4. What tools, people, or workarounds do you currently rely on?
5. Have you tried any services or apps to help? What happened?
6. If you could fix ONE thing about managing your parent's healthcare, what would it be?
7. What would make you hesitant to let someone else handle some of this, even if it freed up your time?

**Key Finding:** The hypothesis was "time burden." The actual finding was
"structural authority gap" - every persona described being the unpaid case
manager stitching together a healthcare system that refuses to talk to itself.

**Specific pains surfaced:**
- Portal fragmentation (different login for every provider)
- Prior authorisation ping-pong
- HIPAA purgatory (legal auth signed but not visible in systems)
- Friday 4pm fires (hospital discharge with nothing arranged)
- 2am worry spiral

---

## Phase 2: Deep Dive

**Goal:** Convert Phase 1 insight into design constraints.

**Group:** 10 new personas, same demographics.

**Questions (informed by Phase 1 findings):**
1. What does "authority to act" mean to you in the context of your parent's healthcare?
2. What would a third party need to demonstrate before you'd grant them HIPAA access?
3. What specific tasks would you trust a coordinator to handle? What's off-limits?
4. How do you want to be kept informed? What level of detail? How often?
5. What trigger moment would make you pick up the phone and hire someone?
6. What's your biggest fear about handing over any control?
7. What would make you fire the coordinator immediately? What's the line?

**Key Findings:**
- Start with HIPAA only (power of attorney is too much trust too fast)
- Named person, not a team (every persona rejected rotating staff)
- Phone and paper first (target demo rejected app-based solutions)
- Guardrails are non-negotiable (spending caps, defined scope, easy exit)

---

## Phase 3: Concept Test

**Goal:** Validate positioning, pricing, purchase intent.

**Group:** 10 new personas, same demographics.

**Questions (informed by Phase 1+2 findings):**
1. Which positioning grabs you most?
   A) "Stop being the unpaid case manager."
   B) "A named coordinator who actually gets things done."
   C) "The family member you wish you had nearby."
   D) "Healthcare is broken. We're the fixer."
2. What's your reaction to $175/month for routine coordination?
3. What about $325/month for complex needs?
4. Would you use a $125 per-event crisis add-on?
5. What moment would trigger you to sign up?
6. What would make you recommend this to a friend or sibling?
7. What would kill the deal, even if everything else sounds perfect?

**Results:**
- **Positioning winner:** "Stop being the unpaid case manager" (validates
  experience without patronising)
- **Pricing:** 100% acceptance at $175 and $325 tiers
- **Purchase trigger:** Hospital discharge (Friday 4pm call)

---

## What Was Produced from 3 Studies

1. **Landing page** - complete, conversion-optimised, copy from persona responses
2. **Pitch deck** - 14 slides, market sizing ($470B TAM), validated pricing
3. **Messaging guide** - brand voice, words to use/avoid
4. **Design specification** - production-ready

---

## Patterns to Reuse

1. **Phase 1 surfaces the real problem** - often different from expected
2. **Phase 2 converts insight into design constraints** - don't jump to solutions
3. **Phase 3 validates the commercial model** - positioning, pricing, triggers
4. **Each phase's questions could not have been designed without the previous phase's findings**
5. **Negative findings save months** - the rejection of apps, emotional positioning, and rotating teams each prevented building the wrong thing
