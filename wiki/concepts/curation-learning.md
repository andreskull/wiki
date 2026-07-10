---
type: concept
title: "Curation learning (instrument resolver)"
product: finfluencer-trade
project: gor_dagster
created: 2026-07-07
updated: 2026-07-07
tags: [instrument-resolution, curation, bigquery, dagster]
---

# Curation learning (instrument resolver)

## Definition

Resolver policies that learn from past human instrument curations so repeat mentions
with different name phrasings auto-resolve instead of re-entering the human queue.

Three policies shipped **2026-07-07**:

| Policy | What | Flag |
|--------|------|------|
| **P3** | Fund-noise second pass in `compute_instrument_name_similarity` | none (always on) |
| **P2** | Unique-ticker acceptance bar **0.85** (spoken ticker → exactly one FI) | `enable_unique_ticker_bar` |
| **P1** | Stage **0.75** promoted-ticker lookup (≥2 manual curations, no alias conflicts) | `enable_ticker_promotion` |

Production: both flags **True** in `potential_prediction_resolution_sensor`.

## Relevance

Finfluencer.trade curators were repeatedly resolving the same tickers (MAA, IJR, etc.)
because `InstrumentAlias` only matched exact `(ticker, name)` strings. Curation
learning compounds human work into ticker-level and alias-level memory without
lowering precision — back-test showed **zero new wrong links** vs baseline.

## Stage order (instrument cascade)

0 → 0.5 (alias) → **0.75 (promotion)** → 1 (ticker + JW, P2 bar) → 2 (name-only) → 1.5 (historical ticker) → 3 (OpenFIGI).

LLM-inferred tickers keep 0.95 bar; P1/P2 never apply.

## Cleanup

One-off manifest adjudication (`configs/alias_audit_manifest.json`) deleted 47 poison
aliases and reopened 219 PP rows. Gap-fix scripts for wrong_link rows without
`alias_id` (X/US Steel, staging re-accepts).

## Projects using this

- [[projects/gor_dagster]] — `instrument_resolution.py`, `instrument_name_similarity.py`, sensor config

## Related concepts

- [[concepts/resolution-pipeline-efficiency]] — JW matcher, re-attempt, alias-on-resolve baseline
- [[concepts/actionable-signal]] — instrument must be `resolution_status='resolved'`

## Sources

- [curation-learning.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/curation-learning.md)
- [instrument-resolution-reference.md](file:///Users/andreskull/gor_dagster/docs/architecture/instrument-resolution-reference.md)
