---
type: project
title: "finfluencer-tracker"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-08-04
tags: [finfluencer, auth, landing, vercel, supabase, react, conversion, seo, linkedin, stripe, entitlement, charts, compare, export]
---

# finfluencer-tracker

Part of [[products/finfluencer-trade]]. Vite/React SPA on Vercel — auth, Stripe billing, product UI, and marketing landing at **`https://finfluencers.trade`**.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Vite + React + TypeScript SPA on **Vercel**: auth, Stripe billing, logged-in product (signals, leaderboard, instruments, onboarding), and **marketing landing** routes in the same deploy. Browser talks to **Supabase** (Auth, Postgres, Edge Functions); pipeline analytics originate in **BigQuery** ([[projects/gor_dagster]]) and reach the app via Supabase sync (see [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md)).

## Current status (2026-08-04)

**Cumulative performance charts shipped** — profile vs S&P, `/compare` head-to-head, Play animation, watermarked PNG/JPEG/MP4 export. See [[concepts/cumulative-performance-charts]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md).

**Subscription entitlement SSOT live on production** — profile tier is the only app entitlement source; Stripe is billing-only with webhook + reconcile self-heal; combined-performance base-table loophole closed; H9 client self-upgrade closed. See [[concepts/subscription-entitlement-ssot]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md).

**LinkedIn on profiles:** Supabase `finfluencers.linkedin_url` is populated from [[projects/gor_dagster]] trusted provenance only. See [[concepts/linkedin-enrichment]].

**CNBC IPO scoreboard live** at **`/cnbc-ipo`** — public, no auth; Supabase RPC `get_ipo_scoreboard_page`.

**Public conversion funnel shipped (2026-07-10).** Anonymous visitors browse `/leaderboard` and `/shows` without login; profile depth gated behind free Spectator signup; Trader for non-featured finfluencer data.

## Access model

| Surface | Anonymous | Spectator | Trader |
|---------|-----------|-----------|--------|
| `/cnbc-ipo` (IPO scoreboard) | Public | Public | Public |
| Leaderboards (`/leaderboard`, `/shows`) | Public (masked columns same as Spectator) | Unchanged | Unchanged |
| `/upgrade` (Pricing) | Public | Unchanged | Unchanged |
| `/compare` | Login + entitlement on locked subjects | Free list; locked → upgrade | Full picker |
| Finfluencer profile (incl. cumulative chart) | Teaser + free-account gate | Featured-3 full; others Trader-gated | Full |
| Show profile | Teaser + free-account gate | Full | Full |
| `/signals`, `/settings`, … | Login required | Login required | Login required |

Monetization stays at **Spectator → Trader column masking**, not a login wall on the leaderboard. **Entitlement for masking/RLS** reads `user_profiles.subscription_tier` after Stripe→profile sync ([[concepts/subscription-entitlement-ssot]]).

## Tech stack

- **Frontend:** React 18, Vite, Tailwind, shadcn/ui, TanStack Query, React Router
- **Hosting:** Vercel (SPA + `api/` serverless for OG meta, sitemap)
- **Supabase (PostgreSQL):** two separate projects — **production** and **development**
- **Payments:** Stripe hosted Checkout + portal; webhooks via Supabase Edge Functions (`stripe-webhook`, `create-checkout`, `check-subscription`)
- **Pipeline data:** production **BigQuery** ([[entities/bigquery]]); app reads Supabase mirror only

## Architecture docs (source of truth)

In-repo: **[WIKI.md](file:///Users/andreskull/finfluencer-tracker/WIKI.md)** and **`docs/architecture/`**:

| Doc | Contents |
|-----|----------|
| [README](file:///Users/andreskull/finfluencer-tracker/docs/architecture/README.md) | Index |
| [system-overview](file:///Users/andreskull/finfluencer-tracker/docs/architecture/system-overview.md) | Stack, Vercel, routing |
| [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md) | Supabase prod/dev, BQ → app path, entitlement SSOT summary |
| [cumulative-performance-comparison-charts](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md) | Cumulative % charts, `/compare`, Play, export (**2026-08-04**) |
| [daily-marks-plan](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md) | Parked: true daily portfolio marks (future) |
| [subscription-entitlement-ssot](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) | Profile SSOT, reconcile, RLS close (**2026-07-27**) |
| [landing-conversion-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) | Public funnel, SEO, anon RPC pattern (**2026-07-10**) |
| [feedback-system](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) | `/feedback` roadmap (**2026-06-02**) |

Cross-subdomain auth: [auth-sharing-landing-app.md](file:///Users/andreskull/finfluencer-tracker/docs/auth-sharing-landing-app.md).

## Completed features

| Date | Feature | Permanent doc |
|------|---------|---------------|
| 2026-08-04 | Cumulative performance comparison charts | [cumulative-performance-comparison-charts.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md) |
| 2026-07-27 | Subscription entitlement SSOT | [subscription-entitlement-ssot.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) |
| 2026-07-23 | LinkedIn URL on finfluencer profiles (trusted sync) | Cross-repo: [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md) |
| 2026-07-11 | CNBC IPO scoreboard (`/cnbc-ipo`, SPCX v1) | Cross-repo: [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) |
| 2026-07-10 | Landing page & conversion funnel improvements | [landing-conversion-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) |
| 2026-06-02 | Custom feedback & public roadmap | [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) |

## Key architecture decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-08-04 | Compare S&P optional (default off); plan-locked picks refused at picker | Head-to-head first; avoid dead-end dual-upgrade selection |
| 2026-08-03 | Holding period ≠ chart lookback (two orthogonal controls) | Book selection vs viewport; both URL-synced |
| 2026-08-03 | Shared `chartExportFrame` for PNG and every MP4 frame | Pixel parity; MP4 needs silent AAC + end-hold + AAC tail-pad |
| 2026-07-30 | Waypoint-anchored SPY-shaped intra-window path (`eq_weight_chainlinked_waypoint_shaped_v3`) | Cuts mean abs error vs linear; still approximate — not for drawdown/vol |
| 2026-07-30 | Benchmark when flat (idle days earn S&P, not cash) | Removes cash-drag between picks; product copy must disclose |
| 2026-07-27 | Profile is app entitlement SSOT; Stripe billing-only | RLS cannot call Stripe; dual truth caused Trader UI + Spectator data |
| 2026-07-27 | `check-subscription` reconciles then returns profile | Self-heals missed webhooks; no Stripe-only unlock / no union |
| 2026-07-27 | Close combined-performance base SELECT; security-definer masked views | Public all-rows leaderboard without Spectator paid-metric leak |
| 2026-07-27 | `past_due` grace + CAPE keep trader until period end | Match Stripe dunning / cancel-at-period-end |
| 2026-07-23 | Profile LinkedIn from trusted sync only | Provisional discovery URLs never reach Supabase; [[concepts/linkedin-enrichment]] |
| 2026-07-11 | IPO scoreboard public route `/cnbc-ipo` | Dedicated cohort page; not gated; BQ→Supabase snapshot via RPC |
| 2026-07-10 | Leaderboards public; free-account gate at profile depth | Login wall hid SEO/marketing value; revenue is Spectator→Trader masking |
| 2026-07-10 | Googlebot → SPA; social bots → `api/og-meta` | Empty-body og-meta pages break search indexing of public content |
| 2026-07-10 | Anon aggregates as `SECURITY DEFINER` RPC + 30s timeout | Project `anon` role has 3s `statement_timeout`; plain views on large tables fail |
| 2026-06-02 | Native feedback board replaces Featurebase | In-app `/feedback` with Supabase + Resend moderation |

## Supabase notes (app layer)

- **Entitlement:** `user_profiles.subscription_tier` (+ status, `stripe_customer_id`, `cancel_at_period_end`, `subscription_reconciled_at`); writers are service-role edge functions only (H9); `get_user_tier()` drives RLS / masked views
- **`finfluencer_combined_performance_public`** + **`leaderboard`** — security-definer masked views; base table not readable by anon
- **`benchmark_daily_prices`** — SPY daily adj_close (synced from BigQuery `PriceHistory`)
- **`get_cumulative_performance_series(...)`** — security-definer RPC; entitlement mirrors TierGate / `is_free_tier` ([[concepts/cumulative-performance-charts]])
- **`get_ipo_scoreboard_page(p_ticker)`** RPC — CNBC IPO scoreboard payload
- **`landing_stats()`** RPC — landing stats bar
- **`show_summary_stats()` / `show_distinct_ticker_counts()`** — guest `/shows` columns
- Promotion: Preview → development Supabase + Stripe test; production Vercel → production Supabase + live Stripe

## MVP and planning docs (cross-repo)

Shipped MVP scope: [`gor_dagster/docs/MVP_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md). Post-MVP backlog: [`INCR_01_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md). Operations: [`finfluencers-app-runbook.md`](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md). Growth: [`gor-blog/growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md). Full table: [[products/finfluencer-trade]] § Planning and strategy.

## Deferred / out of scope

- Terminal product SKU / Terminal downward-reconcile product map
- Magic-link / OTP deliverability (separate initiative)
- Email capture / newsletter on landing
- Embedded Stripe Payment Element migration
- IPO scoreboard access model changes
- True daily portfolio marks (parked — [daily-marks-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md)); show↔show compare; non-S&P benchmarks

## Wiki sync

Vault indexes **WIKI.md** and all of **`docs/`** except **`docs/features/`**. Run **`wiki sync finfluencer-tracker`** after substantive doc updates.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/signal-performance]]
- [[concepts/linkedin-enrichment]]
