---
type: concept
title: "Subscription entitlement SSOT"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-07-27
updated: 2026-10-30
tags: [stripe, supabase, rls, billing, spectator, trader, entitlement, prediction-scores]
---

# Subscription entitlement SSOT

## Definition

**Entitlement SSOT** for finfluencers.trade (`user_profiles.subscription_tier` is the single app entitlement source of truth; expanded to the **Option 2 3-tier gating model** on **2026-10-30** alongside [[concepts/prediction-scores]]):

| Concern | Source of truth |
|---------|-----------------|
| Billing (checkout, invoices, cancel) | Stripe |
| App product unlock + Postgres RLS / SECURITY DEFINER RPCs | `user_profiles.subscription_tier` (`get_user_tier()`) only |
| **Guest (`anon`)** | 3 `is_free_tier` finfluencers unmasked on aggregate leaderboards/profiles (locked rows nulled before `ORDER BY`/`ROW_NUMBER()`); public `/signals` shows the 3 featured finfluencers' fresh picks with `stars`/`explanation_text`/`context_line` masked (`NULL`) behind a free-account CTA |
| **Spectator (free signed-in)** | Unlocks 100% of aggregate track records across all 84 finfluencers and 14 shows (`1W`–`1Y`), plus `1★–5★` Prediction Scores and `"Why N stars?"` explanations for the 3 featured finfluencers |
| **Trader (€19/mo)** | Unlocks all fresh `1★–5★` scored picks and exits (sessions 1–10) across all 84 finfluencers |
| **Raw historical picks (> 10 sessions)** | Client `SELECT` revoked on `signals`, `signal_performance`, `signal_current_performance`, and `pick_scores` for `anon` and `authenticated`; served only through `get_fresh_scored_picks` and aggregate views/RPCs |

Stripe syncs into the profile via edge functions (`stripe-webhook` primary; `check-subscription` reconcile/self-heal; `create-checkout` persists `stripe_customer_id` early). The client (`useSubscription`) unlocks paid UI from the **post-reconcile profile tier**, never from a live Stripe poll alone.

Shared rules live in `shared/entitlement.ts` (trialing and `past_due` keep paid tier; CAPE keeps trader until period end; terminal exempt from downward reconcile without a Terminal product map).

## Relevance

Dual truth (Stripe Trader in UI + Spectator RLS) left paying users on free-tier picks. Closing that gap is release-critical for monetization. Also closes the combined-performance base-table loophole so Spectators cannot SELECT unmasked paid metrics while public leaderboards still show all rows via security-definer masked views.

## Which projects use it

- [[projects/finfluencer-tracker]] — app, edge functions, RLS, migrations (shipped **2026-07-27**)
- Product surface for Spectator→Trader masking on [[products/finfluencer-trade]]

## Related concepts / sources

- Permanent doc: [subscription-entitlement-ssot.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md)
- Summary: [data-layer.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md)
- Entitlement suite: `npm run test:entitlement` in finfluencer-tracker

## Related pages

- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
- [[concepts/actionable-signal]]
- [[concepts/signal-performance]]
- [[concepts/finfluencer-ticker-pick-lookup]] — lookup sits inside `TierGate`; definer RPC re-applies `get_user_tier()`
