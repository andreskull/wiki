---
type: concept
title: "Proof-segment speaker resolution"
product: finfluencer-trade
project: gor_dagster
created: 2026-07-01
updated: 2026-07-01
tags: [speaker-resolution, proof-segments, actionable-signal, supabase, finfluencer]
---

# Proof-segment speaker resolution

Links each **evidence quote** inside a financial signal to a verified `Finfluencer`, gates `ActionableSignal` until all proof-segment speakers are resolved, and surfaces canonical **display names** at Supabase sync time (not stored in BigQuery).

## Problem

Facts extraction stores raw LLM speaker strings on each `proof_segments[]` entry (e.g. `"Jim"`, `"Unknown-4"`, `"Frank Holland"`). Prediction-level speaker resolution only verified the **predictor** — quote attributions could still show wrong or inconsistent names on signal cards.

## Data model

| Field | Where | Purpose |
|-------|-------|---------|
| `proof_segments[].speaker` | `PotentialPrediction` | Raw LLM name (preserved forever) |
| `proof_segments[].finfluencer_id` | `PotentialPrediction` | FK to `Finfluencer` when resolved |
| `proof_segments_speaker_status` | `PotentialPrediction` | `pending` / `partial` / `resolved` |
| `display_name` | Supabase `signals.proof_segments` JSON only | Resolved from `FinfluencerNameVariant` at `mat_signals` build |

**Rule:** `display_name` is never persisted on `PotentialPrediction`. Name changes propagate on the next `sync/mat_signals` + `sync/sync_to_supabase`.

## Resolution order (live + backfill)

1. Proof segment speaker matches prediction `speaker_raw` → use prediction's `finfluencer_id`.
2. Name matches `FinfluencerNameVariant` / display name → use that finfluencer.
3. Single-token unmatched name → **Anonymous Speaker** sentinel (`aaaaaaaa-…`).
4. Multi-token unmatched → leave null; surface in dashboard curation queue.

**Sentinel finfluencers** (`Unknown` `ffffffff-…`, `Anonymous` `aaaaaaaa-…`) use `Finfluencer.status = 'rejected'`. `mat_signals` sets `display_name = NULL` for sentinel IDs so the Tracker falls back to raw `speaker`.

## ActionableSignal gate

Every row in [[concepts/actionable-signal]] requires `proof_segments_speaker_status = 'resolved'`. **No created_at cutoff** — historical and new signals use the same rule. Curators resolve secondary speakers via the unified dashboard queue; signals reappear after all segments are resolved.

## Production state (2026-07-01)

- **73,261** predictions fully resolved; **42** long-tail `partial`/`pending` (multi-token speakers).
- **0** ActionableSignal gate violations.
- **58,263** `mat_signals` rows; Supabase sync verified with `display_name` on proof segments.

## Ops scripts

- `scripts/proof_segment_integration_checks.py` — read-only gate + status checks
- `scripts/verify_mat_signals_proof_segments.py` — spot-check BQ `mat_signals` JSON
- `scripts/verify_supabase_proof_segments.py` — spot-check Supabase `signals`
- `scripts/count_actionable_missing_reasoning_summary.py` — pre-sync reasoning_summary gate

Dagster assets: `proof_segment_speaker_backfill`, `reasoning_summary_backfill`, `sync/mat_signals`, `sync/sync_to_supabase`.

## Projects using it

- [[projects/gor_dagster]] — pipeline implementation
- `finfluencer-tracker` — `EnhancedSignalCard.tsx` renders `display_name || speaker`

## Related pages

- [Feature doc](file:///Users/andreskull/gor_dagster/docs/architecture/features/proof-segment-speaker-resolution.md)
- [[concepts/actionable-signal]]
- [[concepts/speaker-attribution]]
- [[concepts/resolution-pipeline-efficiency]]
- [Speaker Resolution Workflow](file:///Users/andreskull/gor_dagster/docs/operations/speaker-resolution-workflow.md)
