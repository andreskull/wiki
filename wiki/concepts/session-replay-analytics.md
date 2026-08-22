---
type: concept
title: "Session replay analytics"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-08-18
updated: 2026-08-22
tags: [clarity, session-replay, consent, masking, ga4, marketing]
---

# Session replay analytics

## Definition

Microsoft Clarity on `https://finfluencers.trade` so paid-traffic drop-off can be **watched**, not only counted in GA4. Same project supplies heatmaps, rage clicks, and dead clicks.

- **Consent** — analytics category, same shape as [[concepts/reddit-ads-conversion-tracking]] / GA4: production host, env id, stored choice wins, EEA fail-closed. No third banner toggle. Withdrawal uses `consentV2` + `consent(false)` with **no page reload**.
- **Masking** — Balanced mode; inputs always masked; Settings page-root `data-clarity-mask`; empty unmask allowlist. Emails, plan, dates, amounts never upload.
- **Credentials** — no init while `#access_token=` / `?code=` is in the URL. Error-only hashes (`otp_expired`) **are** recorded.
- **Counts vs film** — four funnel events also go to GA4 (`signup_method_click`, `magic_link_requested`, `auth_callback_error`, `onboarding_view`). **Not** key events, **never** Google Ads conversions. `sign_up` stays the bidding signal ([[concepts/google-ads-conversion-tracking]]).

Vendor boundary is only `src/lib/clarityReplay.ts`. Env: `VITE_CLARITY_PROJECT_ID` on Vercel **Production only**. Project Finfluencers.Trade (`y4a1uxw9z3`). Retention **30 days** (9 months if favorited).

## Relevance

GA4 and Ads answer “how many.” Clarity answers “what did they actually do.” Weekly review: [session-replay-review.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/session-replay-review.md). Exit-criteria review **2026-09-17**. Microsoft may use collected data under its terms — disclosed in `/privacy` §6.2. Official Clarity↔GA4 OAuth link is **not** enabled; campaign tags (`ft_utm_*`, `ft_rdt_cid`) already live on recordings.

## Which projects use it

- [[projects/finfluencer-tracker]] — shipped **2026-08-18** (masking pass the same day)

## Standing rules

- Specs import Playwright `test` from `e2e/lib/test` so production e2e sets `ft_replay_off` before every `goto`.
- `identify` uses hashed Supabase `user.id`; never pass `friendly-name`.
- `ft_tier` from `useSubscription()`, omitted until signed-in and resolved.
- OAuth vs magic-link in the dashboard is event presence (`magic_link_requested`), not a `ft_signup_method` tag.
- Preview / localhost never load `clarity.ms` (`isProductionAnalyticsHost`).
- Script **injection** is idle-deferred as of the mobile CWV work (2026-08-22); the gate and command shim stay synchronous so no event is lost ([[concepts/core-web-vitals-mobile]]).
- Do not star the four new GA4 names as key events or Ads conversions.

## Related concepts / sources

- Permanent doc: [session-replay-analytics.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md)
- Ops: [session-replay-review.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/session-replay-review.md)
- DPA: `docs/legal/MicrosoftProductandServicesDPA(WW)(English)(September2025)(CR).docx`
- Google counts: [[concepts/google-ads-conversion-tracking]]
- Reddit pixel: [[concepts/reddit-ads-conversion-tracking]]
- Idle injection / LCP: [[concepts/core-web-vitals-mobile]]

## Related pages

- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/reddit-ads-conversion-tracking]]
