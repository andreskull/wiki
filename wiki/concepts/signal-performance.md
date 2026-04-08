---
type: concept
title: "Signal performance (truncation & implicit flip)"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-08
updated: 2026-04-08
tags: [performance, actionable-signal, bigquery, truncation, finfluencer-trade]
---

# Signal performance (truncation & implicit flip)

How finfluencer **calls** ([[concepts/actionable-signal]]) are turned into measured returns over standard horizons (1w, 1m, 3m, 6m, 1y) and SPY-relative alpha, with **position boundaries** so we never attribute returns after the speaker has reversed direction.

## Definition

- **Entry**: Next trading day after air date, at adjusted open (`first_tradeable_session_date` policy — see repo `docs/architecture/performance-methodology.md`).
- **Completed horizon**: A row in `dagster_prices.SignalPerformance` (BigQuery) only if the horizon’s evaluation end falls **before** any boundary that ends the position.
- **Truncation**: If a boundary occurs first, the horizon is **discarded** (not stored). No partial metrics.

## Boundaries (what ends a position)

1. **Explicit**: `close_long` ends a long; `close_short` ends a short.
2. **Implicit (same finfluencer + same instrument only)**:  
   - `start_long` or `hold_long` → ends a prior **short**.  
   - `start_short` or `hold_short` → ends a prior **long**.  
   Effective date of the boundary = `first_tradeable_session_date` of the flip signal.

Implicit closes do **not** apply across tickers (e.g. long AMD does not close long NVDA).

## Where it is implemented

- BigQuery VIEW `SignalCurrentPerformance` — in-repo SQL: [SignalCurrentPerformance.sql](file:///Users/andreskull/gor_dagster/gor_dagster/sql/views/SignalCurrentPerformance.sql) (`implicit_close_signals` ∪ `close_signals` → `all_close_signals`).
- Completed rows: `PERFORMANCE_SQL` in [performance_calculation.py](file:///Users/andreskull/gor_dagster/gor_dagster/assets/performance_calculation.py) (same CTE pattern).
- Canonical write-up: [performance-methodology.md](file:///Users/andreskull/gor_dagster/docs/architecture/performance-methodology.md) (§2.4, §3.1, §4.4).

## Relevance to Andres’s work

- Scorecards and research (e.g. Cramer / Mad Money) must use the same boundary rules or hit rates and alpha are wrong when speakers flip without saying “sell.”
- **2026-04-08** correction: historical rows that violated implicit truncation were removed from BigQuery and Supabase was resynced via `sync/sync_to_supabase`.

## Projects using it

- [[projects/gor_dagster]] — computes and stores performance.
- [[projects/finfluencer-tracker]] — reads mirrored `signal_performance` / current performance from Supabase.

## Related pages

- [[concepts/actionable-signal]]
- [[projects/gor_dagster]]
- [[entities/bigquery]]
- [[wiki/sources/2026-04-07-cramer-mad-money-performance-methodology]] — research note tying methodology to Cramer analysis
