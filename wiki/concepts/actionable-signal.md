---
type: concept
title: "ActionableSignal"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-07-27
tags: [actionable-signal, prediction, facts-extraction, bigquery, view]
---

# ActionableSignal

The final, deduplicated output of the finfluencer.trade pipeline — a structured financial prediction (BUY/SELL recommendation with instrument, timeframe, and confidence) extracted from a finfluencer's content and tracked against market benchmarks.

## Data model

`ActionableSignal` is a **BigQuery VIEW** over the `PotentialPrediction` table, not a table itself. It applies two filters:

1. **FE config priority deduplication** — when multiple facts extraction runs exist for an episode (e.g., during evaluation), only the highest-priority FE config's predictions appear. Canonical order (**`gor_dagster/sql/views/ActionableSignal.sql`**, `fe_priority`, promote **2026-07-27**): **`fe-gem35fl-recursive`** (1) → **`fe-gem31fl-recursive`** (2) → `fe-gpt-5.2` → `fe-gpt-5` → **`fe-dsv4fr-*`** → retained **`fe-grok-4-fast-reasoning*`** → unknown/other. Deploy via `scripts/update_actionable_signal_view.py`.
2. **Proof-segment speaker gate** — only rows with `proof_segments_speaker_status = 'resolved'` are visible. Unresolved secondary speakers in evidence quotes hide signals until curators resolve them via the unified dashboard queue.

## Proof-segment display names

`display_name` on proof segments is **not** stored in `PotentialPrediction`. `actionable_signal_sql.build_mat_signals_staging_sql` joins `finfluencer_id` → `FinfluencerNameVariant` at `mat_signals` build; Supabase receives enriched JSON. Tracker renders `display_name || speaker`. See [[concepts/proof-segment-speaker-resolution]].

## Critical rules

- **Never `DELETE FROM ActionableSignal`** — it's a VIEW. Delete from `PotentialPrediction` directly.
- **No created_at cutoff** — the speaker gate applies to all signals, historical and new.
- Signals reappear automatically after proof segments are fully resolved.
- Preferring a new FE config in `fe_priority` does **not** invalidate prior artefacts — source-priority / historical PP rows remain.

## Pipeline position

Facts Extraction (Stage 5) → `PotentialPrediction` → `ActionableSignal` VIEW → **performance tracking** ([[concepts/signal-performance]]): horizons truncated by explicit or **implicit** opposite-direction signals on the same instrument

## Projects using it

- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]] (via Supabase mats over ActionableSignal)

## Related pages

- [Gemini 3.5 Flash-Lite migration](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md) (FE priority 1 + production FE)
- [DeepSeek-V4-Flash migration](file:///Users/andreskull/gor_dagster/docs/architecture/features/deepseek-v4-flash-migration.md) (historical FE ladder context)
- [[concepts/proof-segment-speaker-resolution]]
- [Proof-segment speaker resolution feature doc](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md)
- [[concepts/signal-performance]]
- [[concepts/speaker-attribution]]
- [[concepts/llm-config-registry]]
- [[concepts/resolution-pipeline-efficiency]]
- [[entities/bigquery]]
