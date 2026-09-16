---
type: concept
title: "User-default holding period"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-16
updated: 2026-09-16
tags: [holding-period, horizon, settings, supabase, preference]
---

# User-default holding period

## Definition

Every scored pick on finfluencers.trade is measured at five holding periods (`1w`, `1m`, `3m`, `6m`, `1y`). A table still has to open on one of them. The product start window is **6 months**. Signed-in users pin a different start in **Settings**; later visits open on that pin. On-page switches stay visit-local. A named `?horizon=` still wins. Guests never persist a preference.

Resolution order: named URL → signed-in `user_profiles.default_holding_period` → system `DEFAULT_HORIZON = '6m'`. URL omit uses the **system** default, not the saved period — a guest opening a copied clean URL gets 6 months.

`NULL` on the column means unset (follow system). Explicit `'6m'` is a pin. Settings is the only writer. Preference `GRANT UPDATE` on that column only — does not reopen H9.

Holding period (`?horizon=`) is the book. Chart lookback (`?lookback=`) is the viewport. Do not mix them.

## Relevance

Unifies the old split start windows (leaderboard 1 month vs profiles/compare 1 year). Deployed **2026-09-16** (schema on both Supabase ledgers; frontend via GitHub `development → main`, never CLI Vercel). Production Settings save verified.

## Which projects use it

- [[projects/finfluencer-tracker]] — shipped and wrapped **2026-09-16**

## Related concepts / sources

- Permanent doc: [user-default-holding-period.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/user-default-holding-period.md)
- Horizon scoring ground truth: [[concepts/signal-performance]]
- Book vs viewport: [[concepts/cumulative-performance-charts]]
- Preference GRANT vs entitlement lockdown: [[concepts/subscription-entitlement-ssot]]

## Related pages

- [[projects/finfluencer-tracker]]
- [[concepts/signal-performance]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/subscription-entitlement-ssot]]
- [[products/finfluencer-trade]]
