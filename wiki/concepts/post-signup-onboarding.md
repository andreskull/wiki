---
type: concept
title: "Post-signup onboarding"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-09
updated: 2026-09-09
tags: [onboarding, auth, routing, ga4, signup, activation]
---

# Post-signup onboarding

## Definition

A newly registered account on `https://finfluencers.trade` is sent to `/onboarding` immediately after a successful auth callback, then to the pending destination (or `/leaderboard`). Returning sign-ins never see the wizard. Skip is visible and measured. The two-question form and `user_onboarding_preferences` schema are unchanged.

The old 24-hour `ProtectedRoute` gate never ran for homepage signups because `/leaderboard` is public. One trigger at the callback replaces it. **Accepted consequence:** abandon or skip is never re-prompted.

## Relevance

Activation (`onboarding_completed`) was unreachable for ad traffic that registered from the homepage. Deployed **2026-08-31** (PR #71). First clean window 1–8 Sep 2026: 29 `sign_up`, 30 `onboarding_view`, 6 completed, 17 skipped. Funnel **counts** live in GA4, not Clarity — Clarity skips credential URLs and idle-defers first init, so `onboarding_view` on the first post-callback route can miss the film ([[concepts/session-replay-analytics]]).

`sign_up` stays the bidding signal ([[concepts/google-ads-conversion-tracking]], [[concepts/reddit-ads-conversion-tracking]]). `onboarding_skipped` is mirrored into Clarity, is not a GA4 key event, and is never an Ads conversion.

## Which projects use it

- [[projects/finfluencer-tracker]] — shipped **2026-08-31**, wrapped **2026-09-09**

## Standing rules

- Detour when `isWithinSignupWindow(user)` and not completed and not already offered. Shared window with `maybeTrackSignUp` — never gate routing on `ft_ga4_signup_<id>` (written during the same call).
- `maybeTrackSignUp` still runs before `navigate`. Order inside it is unchanged.
- Pending dest is `ft_pending_dest_<userId>` in sessionStorage. `AUTH_REDIRECT_KEY` is cleared on every AuthCallback exit.
- Magic-link `emailRedirectTo` carries `?redirect=`; OAuth does not (same-tab). Both Supabase projects need `/auth/callback**` on the allow-list.
- Never consume storage in a render body (`readPendingDest` in `ProtectedRoute`; `consumePendingDest` only in click handlers).
- `useOnboardingStatus` returns `{ completed }` only. Do not add `loading` back — it invites a paint gate and used to blank protected routes on hourly token refresh.
- Effects that only read the id key on `user?.id`, not the `user` object.
- Do not rename `ff_onboarding_completed_<userId>`.
- Preview deploys prove routing only — `trackGaEvent` no-ops on `*.vercel.app`.

## Related concepts / sources

- Permanent doc: [post-signup-onboarding.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/post-signup-onboarding.md)
- Ops: [session-replay-review.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/session-replay-review.md)
- Public `/leaderboard` is why the old gate failed: [[concepts/session-replay-analytics]], [landing-conversion-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md)

## Related pages

- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/session-replay-analytics]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/reddit-ads-conversion-tracking]]
