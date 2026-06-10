---
type: concept
title: "ActionableSignal"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-06-10
tags: [actionable-signal, prediction, facts-extraction, bigquery, view]
---

# ActionableSignal

The final, deduplicated output of the finfluencer.trade pipeline — a structured financial prediction (BUY/SELL recommendation with instrument, timeframe, and confidence) extracted from a finfluencer's content and tracked against market benchmarks.

## Data model

`ActionableSignal` is a **BigQuery VIEW** over the `PotentialPrediction` table, not a table itself. It applies two filters:

1. **FE config priority deduplication** — when multiple facts extraction runs exist for an episode (e.g., during evaluation), only the highest-priority FE config's predictions appear. Canonical order (**`gor_dagster/sql/views/ActionableSignal.sql`**, `fe_priority`): `fe-gpt-5.2` → `fe-gpt-5` → **`fe-dsv4fr-*`** (DeepSeek) → retained **`fe-grok-4-fast-reasoning*`** tiers → unknown/other.
2. **Proof-segment speaker gate** — only rows with `proof_segments_speaker_status = 'resolved'` are visible. Unresolved speaker attributions hide signals until curators resolve them.

## Critical rules

- **Never `DELETE FROM ActionableSignal`** — it's a VIEW. Delete from `PotentialPrediction` directly.
- **No created_at cutoff** — the speaker gate applies to all signals, historical and new.
- Signals reappear automatically after proof segments are fully resolved.

## Pipeline position

Facts Extraction (Stage 5) → `PotentialPrediction` → `ActionableSignal` VIEW → **performance tracking** ([[concepts/signal-performance]]): horizons truncated by explicit or **implicit** opposite-direction signals on the same instrument

## Projects using it

- [[projects/gor_dagster]]

## Related pages

- [DeepSeek-V4-Flash migration](file:///Users/andreskull/gor_dagster/docs/architecture/features/deepseek-v4-flash-migration.md) (FE priority + production ladder context)
- [[concepts/signal-performance]]
- [[concepts/speaker-attribution]]
- [[concepts/resolution-pipeline-efficiency]]
- [[concepts/spec-driven-development]]
- [[entities/bigquery]]
