---
type: concept
title: "Cumulative performance charts"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-08-04
updated: 2026-09-04
tags: [performance, charts, compare, export, supabase, spy, entitlement]
---

# Cumulative performance charts

## Definition

Buffett-vs-Wood style **cumulative % return** surfaces on finfluencers.trade:

- **Profile** — one finfluencer’s equal-weight overlapping stock-picks book vs S&P 500, with Play animation
- **`/compare`** — two finfluencers head-to-head; S&P optional (default off)
- **Export** — watermarked PNG / JPEG / MP4 (shared canvas frame; silent AAC + end-hold + AAC tail-pad)

**Single number truth:** Supabase RPC `get_cumulative_performance_series` + TS reference (`methodology_key = eq_weight_chainlinked_waypoint_shaped_v3`). Holding period (`?horizon=`) selects which book; lookback (`?lookback=`) is viewport only.

Intra-window path is **waypoint-anchored, SPY-shaped** (not linear) — residual ~9.5pp vs true daily marks. Do **not** derive drawdown/vol/Sharpe from this curve. Idle days earn S&P (benchmark when flat), not cash. Parked true-marks plan: [daily-marks-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md).

## Relevance

Primary shareable performance story for growth/social; gated by the same entitlement model as profile depth ([[concepts/subscription-entitlement-ssot]]). Depends on SPY daily sync from BigQuery `PriceHistory` → Supabase `benchmark_daily_prices` ([[projects/gor_dagster]]). Marketing chrome: `/compare` is in the Explore menu on every marketing page (shipped **2026-08-17**).

## Which projects use it

- [[projects/finfluencer-tracker]] — UI, RPC consumer, export (shipped **2026-08-04**); Google Ads harness reuses the same encoder with optional banner/outro callbacks ([[concepts/google-ads-creative-assets]])
- [[projects/gor_dagster]] — SPY `benchmark_daily_prices` sync mapping; ads variant resolve + GCS archive

## Related concepts / sources

- Permanent doc: [cumulative-performance-comparison-charts.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md)
- Horizon scoring ground truth: [[concepts/signal-performance]]
- Entitlement: [[concepts/subscription-entitlement-ssot]]

## Related pages

- [[projects/finfluencer-tracker]]
- [[projects/gor_dagster]]
- [[concepts/signal-performance]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/linkedin-outreach]] — reuses this chart stack for outreach MP4s (headless render)
- [[concepts/google-ads-creative-assets]] — same encoder; ads-only layout + banner/outro stay off for export/outreach
- [[products/finfluencer-trade]]
