# Search QS / landing page plan (2026-09-29)

Problem: Search "Landing page exp." is Below average on every keyword with a QS reading. Lost IS (rank) 86.82%. Ads are Excellent; the pages are the weak link.

## 1. `/leaderboard` copy (draft)

Constraints: `finfluencer-tracker` performance budget applies (this touches `src/pages/Leaderboard.tsx`, an ad page): run `npm run check:budget` before pushing; commit on `development` only. Copy must be in the initial render, not behind data load, so AdsBot sees it.

**H1:** Stock Picks From Finfluencers, Audited Against the Market

**Intro paragraph (static, above the table):**
We track 37,000+ public stock picks from TV shows, podcasts and YouTube and score each one against the S&P 500 or its sector ETF. Use the leaderboard to see whose stock investment calls actually beat the market, and which trading stocks tips held up over time. Free to browse.

**Title (`LEADERBOARD_PAGE_TITLE`):** Stock Picks Leaderboard: Who Beats the Market? | Finfluencers.Trade

**Meta description (`LEADERBOARD_PAGE_DESCRIPTION`):** Audited stock picks from top finfluencers and financial shows: hit rate, alpha vs the S&P 500 and sector ETFs. See whose stock investment tips really beat the market. Free.

Notes: the numbers (37,000+) should come from the same source the ad copy uses, so they stay consistent. Do not stuff keywords beyond these two natural mentions each of "stock picks", "stock investment", "trading stocks".

## 2. Ad group split (Search - Finfluencer Leaderboard)

| New ad group | Keywords (current) | Final URL | Ads |
|---|---|---|---|
| Cramer / named pundits | cramer stock picks, fast money stock picks, [josh brown], [seekingalpha] | `/finfluencer/jim-cramer` (or `/leaderboard` for the non-Cramer names) | existing Cramer RSA |
| Stock picks & tips | "stock picks", stock tips, stock recommendations, "stock recommendations", "best stocks to buy", best stocks to buy, "best stock pickers" | `/leaderboard` | existing leaderboard RSA, headline pinned to "stock picks" |
| Trading / investing | trading stocks, "trading stocks", stock investment, "stock investment", trading investment, "trading investment", trading alpha, [copy-trading] | `/leaderboard` (new copy above) until a dedicated page exists | existing leaderboard RSA, headline pinned to "trading stocks" |

Keep `/leaderboard/sectors` ad only in the "Trading / investing" group; it had 9 clicks and 1 conversion, so watch, do not expand.

Do not move `trading stocks` (Broad) yet: it produced 18 of 25 conversions. Create the new groups first, then move keywords in one edit and watch a week. Keep the paused `[ifundtraders]` paused.

## 3. Measure

- Before: QS components per keyword on 2026-09-29 (Landing page exp. Below average on 5 of 5 rated keywords).
- After: re-pull the Keywords report (QS, Exp. CTR, Ad relevance, Landing page exp.) 14 days after the copy ships; also Search lost IS (rank).
- URL Inspection > Live test on `/leaderboard`: confirm the H1 and intro are in the rendered HTML.
