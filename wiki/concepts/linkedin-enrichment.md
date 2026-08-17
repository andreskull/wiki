---
type: concept
title: "LinkedIn enrichment (trust-tiered)"
product: finfluencer-trade
project: gor_dagster
created: 2026-07-23
updated: 2026-08-11
tags: [linkedin, finfluencer, curation, supabase, dagster, bigquery]
---

# LinkedIn enrichment (trust-tiered)

## Definition

Pipeline + curation system that attaches LinkedIn `/in/{slug}` URLs to `Finfluencer`
records for frontend display — without letting automated discovery write wrong-person
URLs into the product surface.

**Trust tiers (D-5):**

| Tier | Provenance | Syncs to Supabase / tracker? |
|------|------------|------------------------------|
| **Trusted** | Notion seed, curator Accept/Set URL, speaker-create XOR URL | Yes → `finfluencers.linkedin_url` |
| **Provisional** | Historical LLM/search `auto_discovery` | No — BQ only until curator confirms |
| **Confirmed none** | `Finfluencer.linkedin_unavailable_confirmed_at` | No URL; excluded from default discovery |

Discovery (Gemini + Google Search grounding, `linkedin-gemini-flash`) queues
corroborated candidates into `PendingLinkedInResolution` or records
`LinkedInLookupAttempt` only. It never auto-writes `FinfluencerPlatformProfile`.

## Relevance

Published finfluencers (`mat_finfluencers`) need a trusted LinkedIn link or an
explicit “no LinkedIn” confirmation for profile pages. Search-grounded discovery
is cheap but invents vanity slugs; trust tiers + source-page corroboration absorb
that risk. Third-party LinkedIn APIs were evaluated and **rejected** for residual
coverage (2026-07-23) — identity/trust is the bottleneck, not search cost.

## Production outcomes (wrapup 2026-07-23)

- Published cohort: **100%** trusted-or-none (71 URL + 4 confirmed none / 75)
- Lifetime discovery: ~$0.31 / 1,174 attempts
- Residual unpublished missing (~937) stays selective human curation

## Projects using this

- [[projects/gor_dagster]] — discovery, backfill, Dash curation, audit CSV, sync filter
- [[projects/finfluencer-tracker]] — profile page displays synced `linkedin_url`

## Sources

- [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md)
- Ops: [linkedin-audit-csv-agent-instructions.md](file:///Users/andreskull/gor_dagster/docs/operations/linkedin-audit-csv-agent-instructions.md)

## Related pages

- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/linkedin-outreach]] — uses trusted LinkedIn URLs for contact matching when present
- [[entities/bigquery]]
