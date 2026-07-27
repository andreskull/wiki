---
type: concept
title: "Subscription entitlement SSOT"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-07-27
updated: 2026-07-27
tags: [stripe, supabase, rls, billing, spectator, trader, entitlement]
---

# Subscription entitlement SSOT

## Definition

**Option 1 entitlement model** for finfluencers.trade:

| Concern | Source of truth |
|---------|-----------------|
| Billing (checkout, invoices, cancel) | Stripe |
| App product unlock + Postgres RLS | `user_profiles.subscription_tier` only |

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
