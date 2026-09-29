---
type: concept
title: "Information Ratio"
product: finfluencer-trade
project: gor_dagster
created: 2026-09-29
updated: 2026-09-29
tags: [performance, leaderboard, information-ratio, supabase, bigquery]
---

# Information Ratio

The risk-adjusted score on the leaderboard and on finfluencer and show profile cards. It replaced the return-based Sharpe column on 2026-09-29.

## Definition

Mean of each pick's annualized alpha versus the S&P 500, divided by the sample standard deviation of that alpha. Kept picks only (the same set as hit rate and alpha). Rounded to 2 decimals. NULL below 20 kept picks with an alpha, or when the deviation is 0.

It is a cross-section of individual picks, not a portfolio time series. The cumulative chart must not be used to derive it.

Below 0 lagged the market. 0–0.1 beat it inconsistently. 0.1–0.2 is good. Above 0.2 is excellent. The sign matches average alpha because the denominator is positive.

## Where it is computed

- BigQuery helper `information_ratio_sql` in `gor_dagster/utils/information_ratio_sql.py` (`IR_MIN_KEPT_PICKS = 20`).
- Postgres twin in `show_combined_performance`, literal 20, comment naming that constant.
- Frontend copy `IR_MIN_PICKS` in `finfluencer-tracker/src/lib/pickTerminology.ts`.

A parity test pins the two engines. A post-sync drift check reports a stored value that no longer matches the helper and does not block publishing.

The leaderboard header is **IR**. The profile card is **Info Ratio**. A NULL on an unlocked row is "—". A paid finfluencer's ratio stays masked for a spectator. Show ratios are not masked. `signal_count` still counts every pick.

## Relevance to Andres's work

This is the number the site sorts and explains as "how consistently the picks beat the market." Sharpe of raw return rewarded a rising market and is no longer stored.

## Projects using it

- [[projects/gor_dagster]] — computes and syncs it.
- [[projects/finfluencer-tracker]] — leaderboard, profile cards, Methodology, Trader upgrade email.

## Related pages

- [[concepts/signal-performance]]
- [[concepts/actionable-signal]]
- [[concepts/cumulative-performance-charts]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
