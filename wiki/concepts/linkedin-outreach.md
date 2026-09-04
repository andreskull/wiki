---
type: concept
title: "LinkedIn outreach"
product: finfluencer-trade
project: gor_dagster
created: 2026-08-11
updated: 2026-08-11
tags: [linkedin, notion, outreach, charts, manual-send]
---

# LinkedIn outreach

## Definition

Operator workflow that drafts personalized LinkedIn follow-up messages for Notion **LinkedIn Finfluencer Outreach** contacts with `Stage = "Accepted"`. Code writes Notion comments (and optional chart videos); **humans send on LinkedIn** — no LinkedIn API or browser automation.

## Two layers

1. **Intro personalization** — match contact → finfluencer (LinkedIn URL, then fuzzy name); categories no-match / published / unpublished; one intro-draft Notion comment. [intro-personalization.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-outreach-intro-personalization.md)
2. **Performance content** — published: animated cumulative-return MP4 (claim = chart; cascade S&P → Cramer → peer); unpublished: best-pick + `You said:` quote when available. [performance-content.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-outreach-performance-content.md)

## Relevance

Turns accepted LinkedIn connections into founder-curious intros grounded in real track-record data, without automating LinkedIn itself (ToS / risk).

## Which projects use it

- [[projects/gor_dagster]] — matching, templates, eligibility, GCS upload, Notion REST write-back, pipeline runner
- [[projects/finfluencer-tracker]] — chart cascade + Playwright render harness (`render-outreach-charts.mjs`); reuses [[concepts/cumulative-performance-charts]]
- Depends on trusted LinkedIn URLs ([[concepts/linkedin-enrichment]]) and restored quotes ([[concepts/signal-source-quote]])

## Key rules

- Runner: `./scripts/run_linkedin_outreach_pipeline.sh`
- Marker `<!-- finfluencers-trade-intro-draft v3 -->` — strip before paste
- Claim equals chart; praise only for clear S&P outperformance
- Surname gate on multi-token auto-matches
- Download video from Notion (not GCS) when attaching to LinkedIn

Ops: [linkedin-outreach-intro-agent-runbook.md](file:///Users/andreskull/gor_dagster/docs/operations/linkedin-outreach-intro-agent-runbook.md)

## Related pages

- [[concepts/linkedin-enrichment]]
- [[concepts/signal-source-quote]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/google-ads-creative-assets]] — sibling Chromium harness; ads `opsSession` not yet adopted here
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
