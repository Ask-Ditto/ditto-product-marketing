# Ditto for Product Marketing - Claude Code Skill

Run positioning, messaging, competitive, pricing, and launch research
using [Ditto](https://askditto.io)'s 300,000+ synthetic personas -
directly from your terminal via Claude Code.

## What This Skill Does

When installed, Claude Code automatically loads this skill whenever you
ask it to do product marketing research - positioning validation, messaging
tests, competitive analysis, pricing studies, GTM validation, buyer persona
development, or brand tracking.

Claude will:
- Research your product, market, and competitors
- Design studies with proven PMM question frameworks
- Recruit demographically filtered synthetic panels
- Run studies via the Ditto API
- Generate PMM deliverables: battlecards, messaging hierarchy, positioning scorecards, and more

## Installation

### Project-level (recommended for teams)

```bash
# Clone and copy to your project
git clone https://github.com/AskDitto/ditto-product-marketing.git
cp -r ditto-product-marketing /path/to/your/project/.claude/skills/
```

### Personal (all your projects)

```bash
git clone https://github.com/AskDitto/ditto-product-marketing.git
cp -r ditto-product-marketing ~/.claude/skills/
```

## Setup

Get a free Ditto API key (no credit card required):

```bash
curl -sL https://app.askditto.io/scripts/free-tier-auth.sh | bash
```

Or visit: https://app.askditto.io/docs/free-tier-oauth

Set it as an environment variable:

```bash
export DITTO_API_KEY="rk_free_YOUR_KEY_HERE"
```

Free keys (`rk_free_`): ~12 shared personas, no custom filters.
Paid keys (`rk_live_`): full demographic filtering, unlimited studies.

## Usage

Ask Claude Code to do PMM research:

```
"Validate our positioning for [product] against [competitor].
Test with 10 US adults aged 30-50."

"Run a messaging test. Compare these three taglines with
10 personas matching our ICP."

"I need a competitive battlecard for [product] vs [competitor].
Run a competitive perception study first."

"Test pricing for our new feature. Van Westendorp style,
10 personas, B2B SaaS buyers."
```

Or invoke directly:

```
/ditto-product-marketing "positioning validation for [product]"
```

## The 8 PMM Study Types

| Study Type | When to Use |
|-----------|-------------|
| **Positioning Validation** | Testing how positioning lands with target customers |
| **Messaging Testing** | Comparing 3-4 messaging variants |
| **Competitive Intelligence** | Understanding market perception of you vs competitors |
| **Pricing & Packaging** | Validating willingness-to-pay and feature tiers |
| **GTM Validation** | Validating channels, sales motion, outreach strategy |
| **Product Launch** | Pre-launch concept validation or post-launch sentiment |
| **Buyer Persona Development** | Building data-backed personas |
| **Brand Perception** | Tracking brand health and competitive positioning |

Each type has a proven 7-question framework. See `study-templates.md`.

## What's Included

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill - PMM workflow, study types, API essentials |
| `study-templates.md` | 7-question frameworks for all 8 PMM study types |
| `deliverables.md` | What Claude Code generates from each study type |
| `examples/positioning-validation.md` | Full 3-phase worked example with public study links |

## One Study, Many Deliverables

A single 10-persona study produces:

- Positioning scorecard
- Competitive battlecard
- Messaging hierarchy
- Objection handling guide
- Customer quote bank
- Sales one-pager
- Blog article draft

~60 minutes from zero to complete PMM kit. Traditional: 3-6 weeks.

## About Ditto

Ditto maintains 300,000+ AI-powered synthetic personas calibrated to
census data across 15+ countries. EY validated the methodology at 95%
correlation with traditional research. Studies complete in 15-30 minutes.
Traditional equivalent: 4-8 weeks, $10,000-50,000.

## Links

- **Ditto:** https://askditto.io
- **Full API Guide:** https://askditto.io/claude-code-guide
- **Case Studies:** https://askditto.io/case-studies
- **Existing Product Research Skill:** https://github.com/AskDitto/ditto-product-research

## License

MIT
