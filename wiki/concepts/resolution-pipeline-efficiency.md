---
type: concept
title: "Resolution pipeline efficiency"
product: finfluencer-trade
project: gor_dagster
created: 2026-06-10
updated: 2026-06-10
tags: [resolution, instrument-resolution, speaker-resolution, bigquery, dagster, re-attempt]
---

# Resolution pipeline efficiency

How [[projects/gor_dagster]] drains instrument and speaker resolution backlogs on `PotentialPrediction` without precision regressions.

## Problem

Most pending rows were **frozen** behind the queue gate: once a hash entered `PendingInstrumentResolution` or `PendingSpeakerResolution` with `status='pending'`, the auto-resolution loop stopped seeing it. SequenceMatcher also under-scored company names that Jaro-Winkler would match at the same 0.90 cutoff.

## Solution (wrapped 2026-06-10)

1. **JW matcher** — `compute_instrument_name_similarity()` replaces SequenceMatcher; exchange preference is a tie-breaker only (score never exceeds 1.0).
2. **Re-attempt union** — fetch = never-queued actionable ⊎ `reattempt_eligible=TRUE` frozen rows; on success → `auto_resolved`; on fail → in-place counter bump, no duplicate queue row.
3. **Backlog sweep** — `resolution_backlog_sweep` asset dry-runs then marks eligible frozen rows; drain via `potential_prediction_resolution_job`.
4. **Alias-on-every-resolve** — `_write_instrument_alias()` after Layer 1/1.5/2/3 auto-resolves; Layer 0.5 compounding (~93.5% hit rate at wrapup).

## Production thresholds

| Domain | Setting |
|--------|---------|
| Instrument fuzzy | 0.9 (JW scale) |
| Speaker fuzzy auto-resolve | **0.935** |
| Org-conflict override | **1.00** (disabled) |
| Resolver version | `instr-2026.06-jw` |

Threshold lowering (Increment 5) was **not shipped** after dry-run false-merge review.

## Key job

`potential_prediction_resolution_job` → asset `resolve_pending_predictions` (speakers then instruments; `in_process_executor`).

Manual drain config: `instrument_batch_size: 250`, `speaker_batch_size: 1000` — re-run until actionable counts stop decreasing.

## Outcomes (2026-06-09/10)

- **237** instrument hashes auto-unlocked (sweep + drain).
- **18** speaker hashes auto-unlocked (after threshold tuning).
- Frozen PIR **808 → 560**; remaining tail = delisted/M&A + manual curation gaps (bulk-dismiss deferred).

## Ops scripts (permanent)

- `scripts/analyze_resolution_backlog.py` — queue composition, actionable vs frozen
- `scripts/analyze_resolution_drill.py` — matcher regression / JW vs SequenceMatcher drill

Gate and one-time diagnostic scripts were removed at wrapup.

## Related pages

- [resolution-pipeline-efficiency.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/resolution-pipeline-efficiency.md) — permanent feature doc
- [instrument-resolution-reference.md](file:///Users/andreskull/gor_dagster/docs/architecture/instrument-resolution-reference.md)
- [instrument-resolution-bulk-manual-curation.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/instrument-resolution-bulk-manual-curation.md)
- [[concepts/actionable-signal]] — requires `resolution_status` and `speaker_resolution_status` resolved
