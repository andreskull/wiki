---
type: concept
title: "Signal source quote"
product: finfluencer-trade
project: gor_dagster
created: 2026-08-11
updated: 2026-08-11
tags: [facts-extraction, potential-prediction, proof-segments, verbatim, backfill]
---

# Signal source quote

## Definition

`PotentialPrediction.raw_source_quote` is the finfluencer's own words that evidence a stock call. It is derived from attributed `proof_segments` (speaker-correct span), never invented. Governing rule: **blank beats approximate**.

## Relevance

Without quotes, unpublished LinkedIn outreach could not cite a real pick in the contact's words, and product surfaces lose the primary evidence string behind each signal. A March 2026 FE write-path regression left the field NULL on new rows while evidence remained in `proof_segments`; restoration (2026-08-11) fixed forward write, backfilled kept empties, and synced to Supabase.

## Which projects use it

- [[projects/gor_dagster]] — derivation (`signal_source_quote.py`), trim (`qt-dsv4f`), coverage check, backfill CLI, FE mapper
- [[projects/finfluencer-tracker]] — serving `signals` / Recent Picks copy after Supabase sync
- LinkedIn outreach unpublished best-pick prefers quote over `reasoning_summary` ([[concepts/linkedin-outreach]])

## Key rules

- Forward path attributes by `speaker_raw` ↔ segment speaker; backfill by `finfluencer_id` only
- Spans >700 chars may be model-trimmed then **verbatim-gated** (store the source span, never the LLM candidate)
- Declared DDL is NULLABLE; quality owned by `source_quote_coverage` asset check (post-fix floor ~95%)
- Periodic top-up: [signal-source-quote-backfill.md](file:///Users/andreskull/gor_dagster/docs/operations/signal-source-quote-backfill.md)

## Outcomes (wrapup 2026-08-11)

- ~35.8k kept quotes restored; ~$0.38 LLM trim cost
- Attributable coverage **99.967%**; **27,626** Supabase upserts

Permanent doc: [signal-source-quote-restoration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/signal-source-quote-restoration.md)

## Related pages

- [[concepts/actionable-signal]]
- [[concepts/proof-segment-speaker-resolution]]
- [[concepts/linkedin-outreach]]
- [[projects/gor_dagster]]
- [[entities/bigquery]]
