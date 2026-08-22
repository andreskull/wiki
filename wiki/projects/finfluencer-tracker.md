---
type: project
title: "finfluencer-tracker"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-08-22
tags: [finfluencer, auth, landing, vercel, supabase, react, conversion, seo, linkedin, stripe, entitlement, charts, compare, export, outreach, reddit-ads, navigation, clarity, session-replay, feedback, roadmap]
---

# finfluencer-tracker

Part of [[products/finfluencer-trade]]. Vite/React SPA on Vercel — auth, Stripe billing, product UI, and marketing landing at **`https://finfluencers.trade`**.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Vite + React + TypeScript SPA on **Vercel**: auth, Stripe billing, logged-in product (signals, leaderboard, instruments, onboarding), and **marketing landing** routes in the same deploy. Browser talks to **Supabase** (Auth, Postgres, Edge Functions); pipeline analytics originate in **BigQuery** ([[projects/gor_dagster]]) and reach the app via Supabase sync (see [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md)).

## Current status (2026-08-22)

**Feedback board can decline with a public admin note** — fifth status `declined`, mandatory note (table CHECK), `notify_requested_at` intent marker, vote allowlist on both RLS policies. Orphaned posts (`user_id` NULL) disable “Email the submitter” instead of silently skipping. See [[concepts/feedback-roadmap]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md). Living system: [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md).

**Microsoft Clarity session replay live** — Production only, analytics consent, Balanced masking, Settings page-root mask, credential URLs skipped. Weekly review in [session-replay-review.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/session-replay-review.md); exit-criteria **2026-09-17**. See [[concepts/session-replay-analytics]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md).

**Marketing Explore nav live** — header dropdown + mobile hamburger + footer Product column expose Leaderboard, Compare, and Shows on every marketing page (`/`, `/methodology`, `/privacy`, `/terms`). Single source: `src/config/marketingNav.ts` `EXPLORE_LINKS`. [Feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md).

**Reddit Ads pixel live** — consent-gated `PageVisit` + `SignUp` on production; Conversions campaign optimises on `SignUp`; Traffic Max paused. See [[concepts/reddit-ads-conversion-tracking]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md).

**Cumulative performance charts shipped** — profile vs S&P, `/compare` head-to-head, Play animation, watermarked PNG/JPEG/MP4 export. See [[concepts/cumulative-performance-charts]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md).

**Subscription entitlement SSOT live on production** — profile tier is the only app entitlement source; Stripe is billing-only with webhook + reconcile self-heal; combined-performance base-table loophole closed; H9 client self-upgrade closed. See [[concepts/subscription-entitlement-ssot]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md).

**LinkedIn on profiles:** Supabase `finfluencers.linkedin_url` is populated from [[projects/gor_dagster]] trusted provenance only. See [[concepts/linkedin-enrichment]].

**Outreach chart render (2026-08-11):** headless Playwright harness reuses the cumulative-chart stack for Notion-bound MP4s (`scripts/render-outreach-charts.mjs`, `src/lib/outreach*.ts`). Orchestration and Notion write-back live in [[projects/gor_dagster]] — [[concepts/linkedin-outreach]]. No LinkedIn send from the app.

**CNBC IPO scoreboard live** at **`/cnbc-ipo`** — public, no auth; Supabase RPC `get_ipo_scoreboard_page`.

**Public conversion funnel shipped (2026-07-10).** Anonymous visitors browse `/leaderboard` and `/shows` without login; profile depth gated behind free Spectator signup; Trader for non-featured finfluencer data. Persistent marketing chrome to those routes shipped **2026-08-17**.

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
| [reddit-pixel-tracking](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md) | Reddit pixel, consent gate, SignUp conversion (**2026-08-17**) |
| [session-replay-analytics](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md) | Clarity replay, consent gate, masking, funnel events (**2026-08-18**) |
| [public-navigation-discoverability](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md) | Marketing Explore nav to Leaderboard / Compare / Shows (**2026-08-17**) |
| [feedback-decline-with-note](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md) | Declined status, admin note, notify intent marker (**2026-08-22**) |
| [daily-marks-plan](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md) | Parked: true daily portfolio marks (future) |
| [subscription-entitlement-ssot](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) | Profile SSOT, reconcile, RLS close (**2026-07-27**) |
| [landing-conversion-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) | Public funnel, SEO, anon RPC pattern (**2026-07-10**) |
| [feedback-system](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) | Living `/feedback` system: schema, RLS, notify-moderators, admin panel (**2026-06-02**, declined **2026-08-22**) |

Cross-subdomain auth: [auth-sharing-landing-app.md](file:///Users/andreskull/finfluencer-tracker/docs/auth-sharing-landing-app.md).

## Completed features

| Date | Feature | Permanent doc |
|------|---------|---------------|
| 2026-08-22 | Feedback: declined status with admin note | [feedback-decline-with-note.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md) |
| 2026-08-18 | Session replay & behavioural analytics | [session-replay-analytics.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md) |
| 2026-08-17 | Public navigation discoverability | [public-navigation-discoverability.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md) |
| 2026-08-17 | Reddit pixel tracking | [reddit-pixel-tracking.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md) |
| 2026-08-04 | Cumulative performance comparison charts | [cumulative-performance-comparison-charts.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md) |
| 2026-07-27 | Subscription entitlement SSOT | [subscription-entitlement-ssot.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) |
| 2026-07-23 | LinkedIn URL on finfluencer profiles (trusted sync) | Cross-repo: [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md) |
| 2026-07-11 | CNBC IPO scoreboard (`/cnbc-ipo`, SPCX v1) | Cross-repo: [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) |
| 2026-07-10 | Landing page & conversion funnel improvements | [landing-conversion-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) |
| 2026-06-02 | Custom feedback & public roadmap | [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) |

## Key architecture decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-08-22 | `notify_requested_at` intent marker, not `oldStatus !== newStatus` | A status-only gate can't express "notify with no status change" or "status changed, don't email"; the trigger fires on column mention, the edge function's own value comparison decides whether to send |
| 2026-08-22 | Vote insert/delete narrowed to `under_review`/`planned` allowlist on **both** policies | Closed a pre-existing gap (API allowed votes on any status; only the UI hid the control) at the same time `declined` was added, rather than patching declined in isolation |
| 2026-08-22 | Disable “Email the submitter” when `user_id` is null; do not hide it | Most production posts are orphaned (`ON DELETE SET NULL`); a checked box that cannot send trains the admin to distrust the control |
| 2026-08-18 | Clarity is the sole vendor module; `isClarityAllowed` matches Reddit’s allow-check shape | Same host / env / stored-choice / EEA fail-closed gate as GA4 and the pixel |
| 2026-08-18 | Settings recorded but page-root masked; no SPA pause API | Clarity cannot stop mid-document; visit/clicks stay, email/plan/amounts never upload |
| 2026-08-18 | Four signup funnel events in GA4; six replay-only in Clarity; empty unmask allowlist | 30-day film vs durable counts; new events are never Ads conversions |
| 2026-08-18 | No page reload on analytics withdrawal | `consentV2` denied + `consent(false)`; gate blocks every later load |
| 2026-08-17 | Marketing Explore destinations live in `EXPLORE_LINKS` only | Header, mobile, and footer must not drift; do not reuse `AppSidebar` `navItems` |
| 2026-08-17 | Do not `forceMount` the Explore `NavigationMenu` panel | Crawl paths already exist via homepage CTAs + footer; force-mounting hides a duplicate nav from screen readers |
| 2026-08-17 | Hero teaser NAME column: trim `lg` fixed tracks, keep `1.2fr 1fr` | 50/50 split costs the h1 a fold line; `min-w-0` + oversized numeric columns was the collapse |
| 2026-08-14 | Reddit `pixel.js` gated in our code; no-choice default matches Google Consent Mode | Reddit ignores Consent Mode; US/UK/CA never see the EEA banner so a click-to-accept gate left ad visitors unmeasured |
| 2026-08-12 | Reddit `SignUp` rides `maybeTrackSignUp`; `conversionId = user.id` | One dedupe path with Google; keeps Pixel + future CAPI collapsible |
| 2026-08-12 | Advanced Matching: privacy clause now, hashed email not sent | Avoid a second privacy review without shipping PII at current volume |
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
- **`feedback_posts`** — statuses include `declined`; `admin_note` + `notify_requested_at`; vote RLS allowlist `under_review`/`planned`; never rewrite `handle_feedback_notification()` ([[concepts/feedback-roadmap]])
- Promotion: Preview → development Supabase + Stripe test; production Vercel → production Supabase + live Stripe. Edge function `notify-moderators` deploys **twice**.

## MVP and planning docs (cross-repo)

Shipped MVP scope: [`gor_dagster/docs/MVP_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md). Post-MVP backlog: [`INCR_01_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md). Operations: [`finfluencers-app-runbook.md`](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md). Growth: [`gor-blog/growth_plan.md`](file:///Users/andreskull/gor-blog/growth_plan.md). Full table: [[products/finfluencer-trade]] § Planning and strategy.

## Deferred / out of scope

- Terminal product SKU / Terminal downward-reconcile product map
- Magic-link / OTP deliverability (separate initiative)
- Email capture / newsletter on landing
- Embedded Stripe Payment Element migration
- IPO scoreboard access model changes
- True daily portfolio marks (parked — [daily-marks-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md)); show↔show compare; non-S&P benchmarks
- Reddit Conversions API + Advanced Matching hashed email (pixel + privacy clause live; [[concepts/reddit-ads-conversion-tracking]])
- Official Clarity↔GA4 OAuth dashboard link (playback URLs in GA4); campaign tags already on recordings ([[concepts/session-replay-analytics]])
- Feedback note history / threading / voter-notify; reconstructing deleted submitter accounts; declined-last ordering must move server-side if the board paginates ([[concepts/feedback-roadmap]])

## Wiki sync

Vault indexes **WIKI.md** and all of **`docs/`** except **`docs/features/`**. Run **`wiki sync finfluencer-tracker`** after substantive doc updates.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[concepts/feedback-roadmap]]
- [[concepts/session-replay-analytics]]
- [[concepts/reddit-ads-conversion-tracking]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/signal-performance]]
- [[concepts/linkedin-enrichment]]
- [[concepts/linkedin-outreach]]
- [[concepts/signal-source-quote]]
