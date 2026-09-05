---
type: concept
title: "Finfluencer ticker pick lookup"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-06
updated: 2026-09-06
tags: [ticker, lookup, supabase, rpc, postgrest, entitlement, profile]
---

# Finfluencer ticker pick lookup

## Definition

On a finfluencer profile, signed-in viewers who can already see pick alpha can look up **any** ticker that person has picked — not only the top-10 Most Picks / Performers / Laggards — and get the same two numbers those lists use: mention count and mean annualized alpha vs S&P at the active horizon / action / regime.

**Single number truth:** one Supabase RPC, `finfluencer_ticker_list`. `p_ticker` NULL → ranked top-N (feeds the three list widgets). `p_ticker` set → single-ticker summary (feeds the lookup). Same `WHERE` / `GROUP BY`; AC2 parity is structural.

The same ship (**2026-09-05**, PR #72) deleted two low-comprehension box-plot charts and retired the objects that served only them (`broken_records_rare_picks_for_*`, `mainstream_vs_off_consensus_for_finfluencer`, `mainstream_tickers_snapshot`).

## Why a server-side RPC

Unbounded PostgREST `signals` selects cap at **1000 rows** with no `ORDER BY`. Ranked counts and means were an arbitrary subset; a client-side lookup would have returned a confident “no picks” for tickers the finfluencer had actually picked (≈ half of Joshua Brown’s tickers at 1m). Featured Calls (`best_worst_calls`) and instrument-header α (`instrument_ticker_summary`, **INVOKER**) were the same class of bug and shipped in the same window.

`signal_performance` now has `UNIQUE (signal_id, requested_horizon)`. The covering index `idx_sigperf_signal_horizon_cover` stays.

## Entitlement

`SECURITY DEFINER` + explicit `get_user_tier()` gate; `GRANT EXECUTE` to `authenticated` only. Do **not** copy `show_ticker_list`’s `anon` grant — show profiles are unlocked, paid finfluencer picks are not.

The control lives **inside** `TierGate`. Guests and Spectators on paid profiles get the existing lock overlay; there is no lookup-local guest CTA (unreachable under `pointer-events-none`). Queries: `enabled: !!user && !isLocked`. This is a **UI/product gate**, not a new anon data boundary — `anon_signals_read` still allows anonymous PostgREST reads of free-tier `signals`.

## Retirement order (pipeline-owned snapshots)

Frontend removal → retire the `gor_dagster` write job → DROP. Any other order 404s live profiles or fails a pipeline write. Do not drop indexes whose migration comments name a retired chart — they serve platform traffic, including this RPC. Keep `show_distinct_ticker_counts`.

`apply_profile_supabase_migrations.py` must not re-apply `0020` / `0029` / `0039` (they recreated the dropped RPCs).

## Relevance

The user-facing ask (Featurebase, 2026-06-02) was “how has this person done on NVDA when it is not in the top 10?” The load-bearing fix is the aggregate: without the RPC the lookup would have lied. Also the rule for any future unbounded `signals` read — go through a server-side RPC, never a raw PostgREST select.

## Which projects use it

- [[projects/finfluencer-tracker]] — UI, RPC, unique constraint, drop migration (shipped **2026-09-05**)
- [[projects/gor_dagster]] — mainstream-snapshot asset/job/schedule removed; apply-script no longer recreates retired RPCs

## Related concepts / sources

- Permanent doc: [finfluencer-ticker-pick-lookup.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/finfluencer-ticker-pick-lookup.md)
- Entitlement: [[concepts/subscription-entitlement-ssot]]
- Horizon scoring: [[concepts/signal-performance]]

## Related pages

- [[projects/finfluencer-tracker]]
- [[projects/gor_dagster]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/signal-performance]]
- [[products/finfluencer-trade]]
