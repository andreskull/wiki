---
type: concept
title: "Under-the-radar pickers"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-11-19
updated: 2026-11-19
tags: [leaderboard, ranking, alpha, supabase, sync, gor_dagster]
---

# Under-the-radar pickers

## Definition

A ranking of finfluencers by how their picks did on stocks few others on the site were talking about at the time. It is not a comparison with popular-stock performance. Someone who does not qualify has no row and no badge.

A stock is an instrument with `asset_class = EQUITY`, minus a short override list (BTC, ETH, SOL, XRP, DOGE, GLD, SLV, SPY, QQQ, IWM, DIA). Share classes of one company count as one stock, through [[concepts/share-class-identity]]. A stock is popular for a month when it is one of the 100 most-mentioned stocks in the 12 calendar months ending with that month. Popularity is judged in the month of the pick.

Someone qualifies at 1M, 3M, or 6M when all of these hold on the kept directional less-popular picks: at least 8 different stocks, at least 10 picks, mean alpha above the S&P 500, hit rate above 50%, median alpha above the S&P 500, and mean alpha still above the S&P 500 after every pick of their single best stock is removed. Qualifiers are ranked by that alpha. 1W and 1Y are not ranked.

## Where it lives

`gor_dagster` builds the ranking once a day as consolidator step 14b (`gor_dagster/utils/under_radar_sql.py`) and writes `dagster_sync.mat_finfluencer_under_radar_ranking`. The daily sync upserts `public.finfluencer_under_radar_ranking`.

Guests read `finfluencer_under_radar_ranking_public`: rank 1 and any free-tier row are open, and every other row keeps its rank only. Any signed-in account sees the full list (`20261028120000`). The badge RPC returns rank and qualifier count to every visitor, including a guest.

The page is `/leaderboard/under-the-radar`. A qualifying profile shows `#{rank} Under-the-radar picker`, linking to that row. `/methodology#under-the-radar-pickers` states the rules. The thresholds in the copy are the same constants as the SQL.

Live on `finfluencers.trade` since 2026-10-13. Wrapped 2026-11-19.

## 30-day read (13 Oct–11 Nov 2026)

The badge click-through can be measured: 1 click against 202 profile views of people on the ranking at the end of the window (0.5%). Opening a person from this page was rarer than from the main leaderboard: 6 opens on 151 page views (4.0%) against 335 opens on 2,709 page views (12.4%). The ranking table keeps only the latest run. A guest can open only the rank-1 name.

## Related pages

- Permanent doc: [under-the-radar-pickers.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/under-the-radar-pickers.md)
- [[projects/finfluencer-tracker]]
- [[projects/gor_dagster]]
- [[concepts/share-class-identity]]
- [[concepts/information-ratio]]
- [[concepts/subscription-entitlement-ssot]]
