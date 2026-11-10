---
type: concept
title: "Share-class identity"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-11-10
updated: 2026-11-10
tags: [ticker, share-class, supabase, rpc, sync, picks]
---

# Share-class identity

## Definition

Seven companies that list more than one share class are one ticker on finfluencers.trade. Alphabet is GOOGL (Class A) and GOOG (Class C). Berkshire Hathaway, Fox Corp, News Corp, Under Armour, Liberty Broadband, and Brown-Forman are the other six.

A pick keeps the ticker it was resolved to, that class's prices, and that class's return. Matching, grouping, and counting use the canonical ticker. A single pick shows its class ("GOOG · Class C"). `/instrument/GOOG` redirects to `/instrument/GOOGL`.

## Where the map lives

The only definition is `gor_dagster/utils/share_classes.py` (`GROUPS`). `mat_share_class_tickers` syncs one row per ticker, canonical included, into `public.share_class_tickers`. The sync is incremental. A full refresh would empty the table for the length of the write, and every helper would fall back to the raw ticker.

`canonical_ticker`, `share_class_members`, and `share_class_map` are `STABLE` and not granted to `anon` or `authenticated`. RPCs that filter use `s.ticker = ANY(share_class_members(p_ticker))`. RPCs that group or count left-join the table on `signals.ticker`. The browser keeps no copy: `/signals` reads `share_class_map` from `get_fresh_scored_picks`, and pick rows read the class fields on the row.

An empty `share_class_tickers` turns the feature off. The frontend has to be live before the sync that fills the 14 rows. Production was filled by a manual `supabase_sync_pipeline_job` on 2026-11-10.

Ticker history and `context_line` use the canonical ticker and the company name ("28 completed Alphabet picks"). The prediction model's own prior-pick features stay on the raw ticker. See [[concepts/prediction-scores]].

## Adding a group

Change `GROUPS`, and change `SHARE_CLASS_COMPANIES` in `Methodology.tsx` (the `/methodology#share-classes` copy). The next sync writes the rows. No migration.

## Related pages

- Permanent doc: [share-class-identity.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/share-class-identity.md)
- [[projects/finfluencer-tracker]]
- [[projects/gor_dagster]]
- [[concepts/prediction-scores]]
- [[concepts/finfluencer-ticker-pick-lookup]]
- [[concepts/actionable-signal]]
- [[products/finfluencer-trade]]
