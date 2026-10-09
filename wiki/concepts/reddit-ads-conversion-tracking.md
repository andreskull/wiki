---
type: concept
title: "Reddit Ads conversion tracking"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-08-17
updated: 2026-10-09
tags: [reddit-ads, pixel, conversion-tracking, consent, marketing]
---

# Reddit Ads conversion tracking

## Definition

Browser Reddit pixel on `https://finfluencers.trade` so Reddit Ads can bid on **`SignUp`**, not clicks.

- **`PageVisit`** — SPA views (plus Reddit’s own init-time page visit). Not a conversion.
- **`SignUp`** — one event per real registration, sharing Google’s `maybeTrackSignUp` 5-minute + `localStorage` dedupe. `conversionId = user.id`.
- **Consent** — `pixel.js` is never injected when advertising is denied. Reddit ignores Google Consent Mode, so the gate is in our code before the script tag.
- **No-choice default** matches Google: load outside the EEA; deny in the EEA / unresolved region. Stored decline always blocks.

Pixel ID `a2_jhxip4hotxq3`. Account Botastico OÜ (`jhxip4hotxq3`). Env: `VITE_REDDIT_PIXEL_ID` on Vercel **Production only**.

## Relevance

Parallel paid channel to [[concepts/google-ads-conversion-tracking]]. Same signup moment, same advertising cookie category, separate pixel. The server Conversions API has sent `SIGN_UP` since **2026-09-24** (`conversion_id = user.id`, so Reddit dedupes it with the pixel). The click id is in `localStorage` for 28 days and also on the magic-link URL, because the mail app opens a different browser. Hashed email, IP, and user agent go to that server call. They are still **not** passed to `rdt('init')`.

## Which projects use it

- [[projects/finfluencer-tracker]] — pixel and consent (**2026-08-17**); server SignUp and `signup_attribution` (**2026-10-09**)

Two-week read (24 Sep–7 Oct 2026): Reddit 7 real Sign Ups, first-party Reddit rows 8, GA4 `reddit / cpc` `sign_up` 7. Counts agreed within ±1. About €20.69 per real signup. The €10/day test budget stays. SignUp match quality was still N/A on 27 Sep.

## Campaigns (as of 2026-08-17)

| Campaign | ID | Objective | Status |
|----------|-----|-----------|--------|
| `Finfluencers.Trade Conversions` | `2570370327896224975` | Conversions → **`SignUp`** | Active |
| `Finfluencers.Trade 1` (Max BETA) | `2566412495010814081` | Traffic | **Paused** |

Do not optimise on `PageVisit`. Traffic objective cannot be edited in place.

## Standing rules

- Declined advertising → **zero** `redditstatic.com` requests, not an inert pixel.
- Call sites never inline Reddit event-name strings (`redditAnalytics.ts` constants).
- Do not pass email or match keys to `rdt('init', ...)`.
- `initRdt()` self-fires `PageVisit`; the SPA route effect also fires on that first render (two events). Leave the internal call in `initRdt()`.
- UK is treated as non-EEA for the no-choice load (`eeaCountries.ts`) — product risk (PECR), not a legal conclusion.

## Related concepts / sources

- Permanent doc: [reddit-pixel-tracking.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md)
- Google parallel: [[concepts/google-ads-conversion-tracking]]
- Consent / GA4 plan still in-flight: `docs/features/conversion-measurement-plan/` (not indexed until wrapup)

## Related pages

- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/session-replay-analytics]] — same production-host / stored-choice / EEA fail-closed allow-check shape
- [[concepts/subscription-entitlement-ssot]]
