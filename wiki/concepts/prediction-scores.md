---
type: concept
title: "Prediction Scores (1★–5★) & Option 2 3-Tier Gating"
product: finfluencer-trade
project: null
created: 2026-10-30
updated: 2026-10-30
tags: [prediction-scores, ml, bigquery, gcs, supabase, entitlement, spectator, trader, signals, dashboard]
---

# Prediction Scores (`1★–5★`) & Option 2 3-Tier Gating

Ex-ante **6-Month Prediction Scores (`1★–5★`)** and evidence-backed **"Why N stars?"** explanations for fresh stock picks (`start_long`, `hold_long`, `start_short`, `hold_short`) and fresh position exits (`close_long`, `close_short`), paired with the **Option 2 3-Tier Entitlement Model (`Guest` → `Spectator` → `Trader`)** across `gor_dagster` and `finfluencer-tracker` (wrapped **2026-10-30**).

## Scoring Model (`blend_w50_v1`) & Walk-Forward Registry

- **Target & Horizon:** Predicts 6-month (`180d`) outperformance vs. S&P 500 (`alpha_vs_spy > 0`) at entry (`first_tradeable_session_date`).
- **Blend Architecture:** 50/50 blend (`0.50 * prob_eb + 0.50 * prob_gbt`) of an Empirical-Bayes (EB) prior model (hierarchical shrinkage on speaker and show historical hit rates + alpha) and a `HistGradientBoostingClassifier` over 24 ex-ante features across 4 signal families (**Track Record**, **Stock Profile**, **Consensus**, **Pick Direction**).
- **Strict No-Lookahead Discipline:** Models train strictly on `is_kept = TRUE` picks whose 6-month evaluation window has fully closed before `train_cutoff_date` (`evaluation_end_date < train_cutoff_date`).
- **Per-Cutoff Active Registry (`[T_k, T_{k+1})`) & Full Walk-Forward Backfill (`2019-01-01` → Present):** Stored in `dagster_shared.scoring_model_versions` with serialized artifacts in `gs://gor-ml-models/prediction_scores/blend_w50_v1/<model_version>.joblib`. Each `train_cutoff_date` ($T_k$) has at most one `status = 'active'` model (`2019-01-01`, `2020-01-01`, `2021-01-01`, `2022-01-01`, `2023-01-01`, `2024-01-01`, `2025-01-01`, `2026-01-01`, `2026-10-28`) and scores strictly the out-of-sample window $T_k \le \text{first\_tradeable\_session\_date} < T_{k+1}$ (enforced by `check_active_scoring_model_preflight`).
- **All Historical Picks Since `2019-01-01` Are Scored in Production:** Both BigQuery `dagster_shared.pick_scores` and Supabase `public.pick_scores` contain the complete walk-forward out-of-sample backfill (**~45.9k scored directional picks from `2019-01-01` to present**, not just fresh picks). Equal-weighted cumulative-return or alpha curves per star bucket (`1★` through `5★` vs. S&P 500 `BBG000BDTBL9`) can be queried directly from `dagster_shared.pick_scores` joined to `dagster_shared.ActionableSignal` and `dagster_prices.SignalPerformance` / `dagster_prices.PriceHistory` without running a separate backtest. Note: while the database stores all historical scores (`2019-01-01`+), the web app UI (`get_fresh_scored_picks`) intentionally exposes individual pick scores only during the 10-session fresh window.
- **Write-Once Daily Scoring & Sync:** Step 14c of `supabase_sync_pipeline` (`score_recent_signals`) scores unscored picks (`entry_date >= 2019-01-01`) in `dagster_shared.pick_scores` (`INSERT` only; existing rows are never overwritten during normal sync), then `dagster_sync.mat_pick_scores` merges into Supabase `public.pick_scores` (verified by post-sync `pick_scores_coverage`).

## Monthly Validation Gate, Candidate Banner & Cutoff Drift Retraining

- **Monthly Schedule (`0 5 1 * *` UTC):** `monthly_prediction_score_training_schedule` trains a new monthly cutoff model (`auto_promote = False`), evaluates it against the prior active model (`evaluate_candidate_vs_active`: `spread_5_minus_1_pp > 0`, `monotonic_violations <= 1`, `auc >= active_auc - 0.02`), and registers it with `status = 'candidate'`.
- **Dashboard Promotion (`/scoring-models`):** When any `status = 'candidate'` model exists, the global header shows a `⭐ Models` badge and `/scoring-models` renders a **Candidate Model Notification Banner** with one-click **🚀 Promote to Active** (`promote_model_version`) and **✕ Dismiss / Retire** (`retire_model_version`) actions.
- **Automatic Cutoff Drift Retraining (`retrain_drifted_cutoff_models`):** When newly onboarded shows or historical backfills add completed 6M picks to a closed historical cutoff $T_k$ exceeding **both** `>= 500` new rows (`DEFAULT_CUTOFF_RETRAIN_MIN_NEW_ROWS`) and `>= 5%` growth (`DEFAULT_CUTOFF_RETRAIN_MIN_NEW_PCT`), the monthly job automatically retrains $T_k$, validates it on $[T_k, T_{k+1})$, promotes it if it passes, and rescores $[T_k, T_{k+1})$ via `rescore_cutoff_window`.

## Freshness Window (`sessions 1–10`) & Evidence Rules

- **Strict 10-Session Window:** Individual picks and scores are visible only for the first 10 US trading sessions after entry (`first_tradeable_session_date <= current_session <= window_end_session`). On session 11, picks roll off `/signals` and profile `<FreshScoredPicksSection />` and contribute exclusively to aggregate track records.
- **Evidence-Backed `"Why N stars?"` Explanations:** Up to 2 reason clauses + 1 caveat clause generated from `dagster_shared.scoring_driver_evidence`, enforcing the **`7 of 9` years stability rule** and **`"heavily traded stocks"`** wording for `stock_dollar_volume_20d`.
- **`context_line` Sample-Size Rule:** Requires `>= 3` prior picks on the same ticker to quote both count and average 6M alpha; `1–2` prior picks emit count only; `0` prior picks emit `""`.

## Option 2 3-Tier Entitlement Model (`Guest` → `Spectator` → `Trader`)

| Surface | Guest (`anon`) | Spectator (Free Account) | Trader (€19/mo) |
| :--- | :--- | :--- | :--- |
| **Aggregate Track Records** (`/leaderboard`, `/leaderboard/sectors`, `/shows`, `/compare`, profile KPIs & charts) | **3 featured (`is_free_tier`) finfluencers** unmasked; remaining 81 masked (`NULL` before `ORDER BY`/`ROW_NUMBER()` so sort order never leaks values) | **Unlocked for all 84 finfluencers & 14 shows** (`1W`–`1Y` alphas, hit rates, IR, sector alpha, `/compare`, ticker lookup) | **Unlocked for all 84 finfluencers & 14 shows** |
| **Fresh Scored Picks (`1★–5★`, sessions 1–10)** (`/signals` & profile `<FreshScoredPicksSection />`) | Public `/signals` shows **3 featured finfluencers' fresh picks** with `stars`, `explanation_text`, and `context_line` masked (`NULL`) behind `"Create a free account to see score"` | **3 featured finfluencers** unmasked with full `1★–5★` stars & `"Why N stars?"`; remaining 81 gated behind Trader CTA | **All 84 finfluencers unmasked** |
| **Historical Individual Picks (> 10 sessions old)** | Revoked (aggregates only) | Revoked (aggregates only) | Revoked (aggregates only) |

## Projects using it

- [[projects/gor_dagster]] — model training, `blend_w50_v1` scorer, BigQuery registry & daily scoring, `/scoring-models` dashboard ([prediction-scores.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/prediction-scores.md), [prediction-scores-model-training.md](file:///Users/andreskull/gor_dagster/docs/runbooks/prediction-scores-model-training.md)).
- [[projects/finfluencer-tracker]] — `/signals` feed, profile `<FreshScoredPicksSection />`, Option 2 3-tier RPC gating (`20261023120000`–`20261030120000`), Methodology section ([prediction-scores.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/prediction-scores.md)).

## Related pages

- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/signal-performance]]
- [[concepts/actionable-signal]]
- [[concepts/information-ratio]]
