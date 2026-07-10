---
type: project
title: "finfluencer-tracker"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-07-10
tags: [finfluencer, auth, landing, vercel, supabase, react, conversion, seo]
---

# finfluencer-tracker

Part of [[products/finfluencer-trade]]. Vite/React SPA on Vercel — auth, Stripe billing, product UI, and marketing landing at **`https://finfluencers.trade`**.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Vite + React + TypeScript SPA on **Vercel**: auth, Stripe billing, logged-in product (signals, leaderboard, instruments, onboarding), and **marketing landing** routes in the same deploy. Browser talks to **Supabase** (Auth, Postgres, Edge Functions); pipeline analytics originate in **BigQuery** ([[projects/gor_dagster]]) and reach the app via Supabase sync (see [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md)).

## Current status (2026-07-10)

**Public conversion funnel shipped.** Anonymous visitors browse `/leaderboard` and `/shows` without login; profile depth is gated behind free Spectator signup; Trader tier unchanged for non-featured finfluencer data. Landing page uses unified leaderboard teaser cards (finfluencers + shows). Mobile UX: in-page sticky search on leaderboard, touch `InfoTip`s, bottom-nav fix. SEO: Googlebot receives SPA; social bots use `api/og-meta`. Supabase prod: anon-safe RPCs for landing stats and show summary columns. Stripe Checkout wallet-ready (Dashboard-configured; do not pin `payment_method_types` in code).

## Access model

| Surface | Anonymous | Spectator | Trader |
|---------|-----------|-----------|--------|
| Leaderboards (`/leaderboard`, `/shows`) | Public (masked columns same as Spectator) | Unchanged | Unchanged |
| `/upgrade` (Pricing) | Public | Unchanged | Unchanged |
| Finfluencer profile | Teaser + free-account gate | Featured-3 full; others Trader-gated | Full |
| Show profile | Teaser + free-account gate | Full | Full |
| `/signals`, `/settings`, … | Login required | Login required | Login required |

Monetization stays at **Spectator → Trader column masking**, not a login wall on the leaderboard.

## Tech stack

- **Frontend:** React 18, Vite, Tailwind, shadcn/ui, TanStack Query, React Router
- **Hosting:** Vercel (SPA + `api/` serverless for OG meta, sitemap)
- **Supabase (PostgreSQL):** two separate projects — **production** and **development**
- **Payments:** Stripe hosted Checkout + portal; webhooks via Supabase Edge Functions (`create-checkout`)
- **Pipeline data:** production **BigQuery** ([[entities/bigquery]]); app reads Supabase mirror only

## Architecture docs (source of truth)

In-repo: **[WIKI.md](file:///Users/andreskull/finfluencer-tracker/WIKI.md)** and **`docs/architecture/`**:

| Doc | Contents |
|-----|----------|
| [README](file:///Users/andreskull/finfluencer-tracker/docs/architecture/README.md) | Index |
| [system-overview](file:///Users/andreskull/finfluencer-tracker/docs/architecture/system-overview.md) | Stack, Vercel, routing |
| [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md) | Supabase prod/dev, BQ → app path |
| [landing-conversion-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) | Public funnel, SEO, anon RPC pattern (**2026-07-10**) |
| [feedback-system](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) | `/feedback` roadmap (**2026-06-02**) |

Cross-subdomain auth: [auth-sharing-landing-app.md](file:///Users/andreskull/finfluencer-tracker/docs/auth-sharing-landing-app.md).

## Completed features

| Date | Feature | Permanent doc |
|------|---------|---------------|
| 2026-07-10 | Landing page & conversion funnel improvements | [landing-conversion-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) |
| 2026-06-02 | Custom feedback & public roadmap | [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) |

## Key architecture decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-07-10 | Leaderboards public; free-account gate at profile depth | Login wall hid SEO/marketing value; revenue is Spectator→Trader masking |
| 2026-07-10 | Googlebot → SPA; social bots → `api/og-meta` | Empty-body og-meta pages break search indexing of public content |
| 2026-07-10 | Anon aggregates as `SECURITY DEFINER` RPC + 30s timeout | Project `anon` role has 3s `statement_timeout`; plain views on large tables fail |
| 2026-07-10 | Unified `LeaderboardTeaserCard` on landing | Same teaser pattern for finfluencers and shows; full explore in app |
| 2026-07-10 | `rankAndFilter`: sort → global rank → filter | Name filter must not renumber rows |
| 2026-06-02 | Native feedback board replaces Featurebase | In-app `/feedback` with Supabase + Resend moderation |

## Supabase notes (app layer)

- **`landing_stats()`** RPC (was view) — landing stats bar; frontend `supabase.rpc('landing_stats')`
- **`show_summary_stats()` / `show_distinct_ticker_counts()`** — composite indexes on `signals` + 30s timeout for guest `/shows` columns
- **`finfluencer_combined_performance_public`** — existing view; no new view for public leaderboard (anonymous → spectator tier)

## MVP and planning docs (cross-repo)

Shipped MVP scope: [`gor_dagster/docs/MVP_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md). Post-MVP backlog: [`INCR_01_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md). Operations: [`finfluencers-app-runbook.md`](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md). Growth: [`gor-blog/growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md). Full table: [[products/finfluencer-trade]] § Planning and strategy.

## Deferred / out of scope (2026-07-10 funnel)

- Magic-link / OTP deliverability (separate initiative)
- Email capture / newsletter on landing (not in funnel feature)
- Embedded Stripe Payment Element migration

## Wiki sync

Vault indexes **WIKI.md** and all of **`docs/`** except **`docs/features/`**. Run **`wiki sync finfluencer-tracker`** after substantive doc updates.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[concepts/signal-performance]]
