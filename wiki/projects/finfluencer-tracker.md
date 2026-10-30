---
type: project
title: "finfluencer-tracker"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-04-06
updated: 2026-10-30
tags: [prediction-scores, signals, rum, web-vitals, mobile, finfluencer, auth, landing, vercel, supabase, react, conversion, seo, linkedin, stripe, entitlement, charts, compare, export, outreach, reddit-ads, navigation, clarity, session-replay, feedback, roadmap, lcp, performance, ga4, bigquery, analytics, google-ads, ticker, lookup, onboarding, holding-period, sector-alpha, sector-leaderboard, supabase-migrations, ledger-baseline]
---

# finfluencer-tracker

Part of [[products/finfluencer-trade]]. Vite/React SPA on Vercel — auth, Stripe billing, product UI, and marketing landing at **`https://finfluencers.trade`**.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Vite + React + TypeScript SPA on **Vercel**: auth, Stripe billing, logged-in product (signals, leaderboard, instruments, onboarding), and **marketing landing** routes in the same deploy. Browser talks to **Supabase** (Auth, Postgres, Edge Functions); pipeline analytics originate in **BigQuery** ([[projects/gor_dagster]]) and reach the app via Supabase sync (see [data-layer](file:///Users/andreskull/finfluencer-tracker/docs/architecture/data-layer.md)).

## Current status (2026-10-30)

**Prediction Scores & Option 2 3-tier gating wrapped (2026-10-30)**: Ex-ante `1★–5★` 6-Month Prediction Scores (`blend_w50_v1`) and evidence-backed `"Why N stars?"` explanations (`7 of 9` years stability rule, `"heavily traded stocks"` wording) + sample-size-gated `context_line` (`>= 3` count + avg, `1–2` count only, `0` empty) live across `/signals` and finfluencer profile `<FreshScoredPicksSection />` (PRs `#116`–`#120`, migrations `20261023120000`–`20261030120000`). Individual picks and fresh exits (`close_long`, `close_short` with `"Was ★★★★☆"`) are shown strictly for the first 10 US trading sessions after entry (`get_fresh_scored_picks`). Direct client `SELECT` on raw `signals`, `signal_performance`, `signal_current_performance`, and `pick_scores` is revoked for `anon` and `authenticated`. Under the **Option 2 3-tier entitlement model**: **Guest (`anon`)** sees 3 `is_free_tier` finfluencers unmasked on aggregate views, free-account signup gates on locked rows with sort-order leakage closed before `ORDER BY`/`ROW_NUMBER()` in `leaderboard` and `get_sector_leaderboard`, and sees the 3 featured finfluencers' fresh picks on public `/signals` with `stars`/`explanation_text`/`context_line` masked to `NULL` behind a `"Create a free account to see score"` badge; **Spectator (free signed-in)** unlocks 100% of aggregate track records across all 84 finfluencers and 14 shows (`1W`–`1Y`) plus `1★–5★` scores and `"Why N stars?"` explanations for the 3 featured finfluencers; **Trader (€19/mo)** unlocks all fresh `1★–5★` scored picks and exits across all 84 finfluencers. `paywall_hit`, `paywall_view`, and `paywall_cta_click` wired across `leaderboard:main`, `leaderboard:sectors`, `leaderboard:under_radar`, `signals:teaser`, `compare`, and `landing:hero_teaser`. Preserved 64 reproducible research scripts/reports in `docs/features/prediction-scores/research/` while purging ~9.10 GB of transient outputs (`keep-research`). See [[concepts/prediction-scores]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/prediction-scores.md).

**Conversion measurement wrapped (2026-10-20)**: Consent Mode, `purchase`, and the engagement events have been live since 2026-08-11. GA4 key events other than `sign_up` are imported to Google Ads as secondary. `sign_up` (€20) stays the only primary. `comparison_created` and `purchase` were not promoted: last 30 days through 2026-10-19 were 99 `sign_up`, 5 `comparison_created`, 0 `purchase`. See [[concepts/google-ads-conversion-tracking]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/conversion-measurement-plan.md).

**Landing tour improvements wrapped (2026-10-20)**: the first-visit tour on `/` is a 50/50 test (`TOUR_ARM_TOUR_SHARE`). `tour_assign` fires in both arms after the same idle delay. Slide 1 is a fixed proof line plus Just browse. Closes, abandons, and time on a slide are recorded, and a sign-up carries the arm and outcome across the magic link. `internal_accounts` is the only test-user list; a listed account skips Google Ads and Reddit SignUp, while GA4 `sign_up` and `signup_attribution` still record it. First read 25 Sep–20 Oct: tour 6/103, none 7/112. The split stays 50/50. Grok and ank read the funnel on the Thursday review. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-tour-improvements.md).

**RUM off GA4 wrapped (2026-10-20)**: since the 2026-10-12 production release, LCP, INP, CLS, TTFB, and DATA_READY go by `sendBeacon` to `/api/rum` and Supabase `web_vitals_events`, not to GA4. Late `web_vitals` hits from frozen iOS tabs had been stretching GA4 sessions (Oct 4–10: 349 s average session against 17 s of engagement). GA4 `web_vitals` is 0 from the release. Paid sessions over 2 hours with under 30 seconds of engagement went from 4 in 8 days (all iOS, all ending in `web_vitals`) to 1 in 202 paid sessions on Oct 13–18; that one is a late `view_page` from a GA4 rule, not `web_vitals`, and Andres accepted it. The GA4 rule was not examined or changed. The ads review's Oct 13 "(not set)" cohort (59 sessions on Oct 12) was GA4 API processing lag, not attribution loss: Oct 18's 43 re-queried to 3, and the BigQuery export never showed it. Production e2e intercepts the beacon (`captureRum()`) and carries the internal-traffic flag, because each unintercepted run had inserted real rows and counted as new GA4 users. The migration went to production before the code. Read the table with `docs/ops/sql/web_vitals_events_p75.sql`; rows before 2026-10-12 stay in BigQuery. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/rum-off-ga4.md) (its limitations list a Vercel recovery for a missed `main` build: Create Deployment → `main` → Deploy to Production).

**Mobile experience uplift wrapped (2026-10-20)**: production 2026-09-23 (`7f2df15`). Below `md` the document scrolls. List pages are one table. The landing page dropped from 14,305 px to 7,888 px at 375 px, and the four-slide first-visit tour shipped here (the arm test is the landing-tour doc above). The 14-day read accepted `/leaderboard` mobile field LCP p75 at 1.80 s against a 1.5 s target. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-experience-uplift.md).

**Signed-out QA findings and data integrity wrapped (2026-10-19)**: 11-area sweep across 38 tasks (`fix-findings-0810`). Committed static snapshots for landing and guest compare charts (`landingShowcase.json`, `compareShowcase.json`); `prebuild` freshness gate fails if > 31 days old; automated Monday anon refresh (`refresh-showcase-snapshots.yml`, $\le 5$ pp threshold). Upstream SPY adjusted price restatement anomaly repaired and verified against Yahoo (+0.0 pp divergence); recurring Dagster drift check wired. Server-side masking SSOT for non-Traders on paid profiles and on shows with $\ge 80\%$ picks from a single paid finfluencer (*The Morning Filter* 100%, *Hedge Fund Tips* 99.5%). 25 explicit routes in `vercel.json` with genuine 404s and `noindex`. Default `/compare` experience prefilled with Jim Cramer vs Josh Brown with inline signup redirect for locked subjects. Unified Q4 pick count predicate (`start_long`, `hold_long`, `start_short`, `hold_short`) with `is_kept` strictly isolated to sector averages. Mobile sign-up uplift: no viewport jumping on `/leaderboard`, sticky locked bar offset below header, flexible Turnstile widget for 320/360px viewports, `signup_cta_click` + GA4 `location` dimension, and non-PII signup rows RPC. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/signed-out-qa-findings-fix.md).

**Mobile sign-up drop closed (2026-10-16)**: mobile new users (`auth.users`) fell from 3.5 to 1.1 a day after deploy `29b315f` on 2026-09-29. The deploy was not the cause: the rate recovered with it in production. About half of the lost Search mobile sign-ups were non-US traffic removed by the 2026-09-28 location change ("Presence only"); Search Partners (6% to 43% of Search clicks, 1 conversion in 36) filled the rest and stalled `/signup` (mobile users on `/auth/callback` per users on `/signup` went 71% to 31% to 84% after partners were switched off on 2026-10-07). The 7 days after that gave 16 mobile new users (2.3 a day) against a geo-adjusted expectation of 15.5; the rollback was never built. The original 3 a day target was not met (2.3); Search mobile 15% was (20%). Reddit mobile conversion stays low (1 on 129 sessions; thin). `paywall_view` and `paywall_cta_click` now measure the paywall to `/signup` step. `sharpe` was dropped on 2026-09-29, so the Information Ratio display could not have been reverted anyway. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-signup-drop.md).

**Search QS landing page wrapped (2026-10-14)**: `/leaderboard` heading is Finfluencers Stock Picks Leaderboard, with "Scored against the market" under it, including in the HTML shell. The 14-day read (30 Sep–13 Oct) put Search lost impression share (rank) at 72.42% against 86.82% on 29 Sep. Both `trading stocks` keywords are Quality Score 8 with landing-page experience Average. The page copy stays. Account edits are a checklist in `docs/ops/search-qs-ad-groups.md`. Leftovers in Reklaamirühm 1 were closed outside this feature. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/search-qs-landing-page.md).

**Ad landing time to content wrapped (2026-10-09)**: The HTML shell starts the guest data request on `/leaderboard`, `/shows`, `/leaderboard/sectors`, and `/finfluencer/:slug`. `nav_kind` splits a cold visit from an in-app one. Sectors closed under 2.5 s. The profile was still over, and the production trace showed `app-core` finishing after the header call, so server HTML stayed closed. Ads landing-page experience on 2026-10-09 was still mostly Below average and is not the gate. See [[concepts/core-web-vitals-field]], [[concepts/core-web-vitals-mobile]], and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ad-landing-time-to-content.md).

**Onboarding skip improvements wrapped (2026-10-06)**: Skip stays a full-width secondary button. Continue shows an inline error and keeps the typed answer. Loading shells on the leaderboard, compare, and profiles do not take taps. The completion bar was missed: 6 of 14 sign-ups finished in 29 Sep–5 Oct (43%; the bar was 50%). Dead clicks after Skip were not re-measured on the skip segment. See [[concepts/post-signup-onboarding]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-skip-improvements.md).

**Supabase ledger baseline wrapped (2026-09-30)**: `supabase/migrations/` now replays from an empty database to production's schema. The 73 old migrations moved to `supabase/migrations_archive/`; the live pair is `20261001000000_baseline_schema.sql` (generated from production with `pg_dump` 17) plus a hand-written idempotent `20261001000100_baseline_supplement.sql` (extensions, `auth.users` trigger, publication membership). Both live histories were repaired with `migration repair`; the catalog snapshot was byte-identical before and after. `npm run db:replay-check` proves a replay equals production (two documented exceptions). Two sector RPC bodies were realigned first (`20260930100000`). `supabase/seed.sql` is local only. New migrations before 2026-10-01 00:01 UTC must be named `20261001000200_…` or later. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/supabase-ledger-baseline.md).

**Information Ratio live (2026-09-29)**: The leaderboard column is **IR** and the profile card is **Info Ratio**. Both read `information_ratio` (NULL below 20 kept picks shows "—"; a gated row stays locked). Methodology defines it. The Trader upgrade email says "Information Ratios" on both deployed `stripe-webhook` functions. See [[concepts/information-ratio]] and [information-ratio.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/information-ratio.md).

**Profile and leaderboard script split wrapped (2026-09-28)**: The show profile reports data-ready on the real header row, and a production trace named data as the late leg. Desktop `/leaderboard` does not have enough samples to name a cause. Supabase realtime and storage stand-ins are on `development` (`e66103c`), not production (`20bb0b3`). The profile body still loads with the first card; the 591,608 byte target was not met. See [[concepts/core-web-vitals-mobile]], [[concepts/core-web-vitals-field]], and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/profile-leaderboard-script-split.md).

**Social share previews wrapped (2026-09-28)**: Crawlers on `/`, `/leaderboard`, `/shows`, `/show/:slug`, `/finfluencer/:slug`, and `/cnbc-ipo` receive HTML from `api/og-meta.ts`. Profiles and the IPO scoreboard use a generated PNG from `api/og-image.tsx`. The other routes use `public/og-image.png`. Record: [social-share-previews.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/social-share-previews.md).

**Onboarding questionnaire v2 live (promoted 2026-09-25, wrapped 2026-09-28)**: `/onboarding` asks one research question per screen — channel, follows, optional names, holding period. Skip still writes nothing. The holding answer does not change the chart pin. Four nullable columns on `user_onboarding_preferences` (migration `20260924180000`, both ledgers). GA4 dimensions for `channel`, `follows`, and `holding_period`; `name_count` is a custom metric; `step` stays Tour Step. The pipeline dashboard Users page shows the answers. See [[concepts/post-signup-onboarding]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-questionnaire-v2.md).

**Investor positioning wrapped (2026-09-27)**: Homepage hero and teasers speak to 1-week to 1-year holds. Cards fill at `DEFAULT_HORIZON` (`6m`). Ads subject period is `6m`; a miss fails as `no_6m_coverage` and does not fall back. Account edits are a UI checklist (`docs/ops/investor-positioning-ads-checklist.md`) — no ads API, no new campaign. Shipped on `development` as `18bda64`. `gor_dagster` `HOLDING_PERIOD = "6m"` was still an uncommitted working-tree edit at wrapup. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/investor-positioning-landing-ads.md).

**Search RSA image assets wrapped (2026-09-27)**: Studio-show stills on Search — Finfluencer Leaderboard (`24038241586`) → Assets → Images. Five Eligible, women square (`search-panel-women-1200x1200`) still Pending — Under review, zero Disapproved. DoD met. Durable copies in `docs/ops/google-ads-search-images/`. PMax harness JPEGs stay off Search (overlay carve-out is PMax-only). No tracker code. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ads-improvements.md).

**Performance palette live (wrapped and promoted 2026-09-25)**: Button and badge fill is `#1f812d` (`--primary`). Positive and negative alpha — text, chart fills, Long/Short badges — are `#51ad57` and `#e36b65` (`--positive`, `--negative`). `text-primary` uses the lighter green. `--destructive` stays error chrome. `color-contrast` is enforced; HowItWorks step numerals are the only exemption (`data-contrast-exempt`). Leaderboard names wrap to two lines; the profile name uses `break-words`. A full local Playwright matrix is not a release gate. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ui-accessibility-and-test-reliability.md).

**Sector leaderboard name filter at every width (wrapped 2026-09-24)**: On `/leaderboard/sectors` the name column header shows the `?q=` filter from the `md` breakpoint up, for finfluencers and shows. It filters rows already loaded. The desktop top bar still opens `/leaderboard`. Visibility is a class on the sector page, not a change to the shared `NameFilterInput`, so `/leaderboard` and `/shows` still hide their filter on a wide screen. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/sector-leaderboard-wide-name-filter.md).

**Sector-relative alpha & sector leaderboards live (wrapped 2026-09-20)**: Interactive `/leaderboard/sectors` across 11 GICS sectors + BTC + SPY; profile `SectorCompositionWidget` with Stock Selection Skill + Sector Allocation Skill = Alpha in Sector; Portfolio Alpha Impact in percentage points (`pp`); Supabase `get_sector_leaderboard` RPC; Methodology sector breakdown. See [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/sector-relative-alpha.md).

**User-default holding period live (wrapped 2026-09-16)** — system start is 6 months on every product view; signed-in users pin in Settings; named `?horizon=` still wins; URL omit uses the system default, not the save. Column `user_profiles.default_holding_period` on both ledgers. See [[concepts/user-default-holding-period]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/user-default-holding-period.md).

**Post-signup onboarding live (deployed 2026-08-31, wrapped 2026-09-09)** — auth-callback detour for fresh signups; Skip measured; 24-hour route gate removed. First clean window 1–8 Sep: 29 `sign_up`, 30 `onboarding_view`, 6 completed, 17 skipped. Counts in GA4, not Clarity. See [[concepts/post-signup-onboarding]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/post-signup-onboarding.md).

**Field Core Web Vitals wrapped (2026-09-08)** — first-party `web_vitals` + `DATA_READY` in BigQuery; **field decides done, lab decides whether a change helped**. `/` LCP met ≤ 2.5 s; `/` teaser DATA_READY missed (~4 s vs 2.0 s — `landing_stats`); lab TTFB 61 ms was an edge HIT (field 701 ms). Freeze lifted. See [[concepts/core-web-vitals-field]], [[decisions/field-authoritative-cwv-2026-09]], [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-field.md).

**Finfluencer ticker lookup live (2026-09-05)** — PR #72 (`a38e360`). Server-side `finfluencer_ticker_list` feeds both the ranked lists and a profile lookup (any ticker, not only top-10). Two box-plot charts retired; `mainstream_tickers_snapshot` pipeline and RPCs dropped after the frontend was live. Production and development Supabase ledgers level at 61. See [[concepts/finfluencer-ticker-pick-lookup]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/finfluencer-ticker-pick-lookup.md).

**Google Ads PMax creatives live (2026-09-04)** — V1 + V4 uploaded from run `2026-09-04T0804Z`. Playwright harness over the shipped chart encoder (`scripts/render-google-ads-assets.mjs`, `src/lib/ads*.ts`). Banner / outro are additive optional callbacks so outreach and on-site export stay byte-identical. Resolve + GCS archive live in [[projects/gor_dagster]]. See [[concepts/google-ads-creative-assets]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/google-ads-creative-assets.md).

**Mobile Core Web Vitals delivery live** — lab LCP under 2.5 s on `/` (2.03 s, was 3.74 s) and most public SPA routes; `/show/:slug` still 2.54 s. Route splitting, self-hosted fonts, immutable `/app-assets` cache, idle third-party injection, fetch-gated profile headers. Lab-vs-field rule **amended 2026-09-08** (field is the done gate). Four of the CrUX URLs are MkDocs — see [[projects/gor-blog]]. Permanent doc: [core-web-vitals-mobile.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-mobile.md). Concept: [[concepts/core-web-vitals-mobile]]. Ops: [core-web-vitals.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/core-web-vitals.md).

**GA4 custom dimensions + BigQuery export (2026-09-02).** Web Vitals parameters arrived populated but were unregistered — Explore read `(not set)`. Registered 10 dimensions + `metric_value`; export to `gurus-on-record`. See [[decisions/ga4-instrumentation-registration-2026-09]], [[concepts/core-web-vitals-field]], [[entities/bigquery]].

**Feedback board can decline with a public admin note** — fifth status `declined`, mandatory note (table CHECK), `notify_requested_at` intent marker, vote allowlist on both RLS policies. Orphaned posts (`user_id` NULL) disable “Email the submitter” instead of silently skipping. Board smoke (`e2e/feedback-smoke.spec.ts`) is **on demand only** since **2026-09-28**: fixture inserts email moderators through Resend, so `npm run test:e2e` ignores the file. Run `npm run test:e2e:feedback` (Chromium, once). See [[concepts/feedback-roadmap]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md). Living system: [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) §6.

**Microsoft Clarity session replay live** — Production only, analytics consent, Balanced masking, Settings page-root mask, credential URLs skipped. Weekly review in [session-replay-review.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/session-replay-review.md); exit-criteria **2026-09-17**. See [[concepts/session-replay-analytics]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md).

**Marketing Explore nav live** — header dropdown + mobile hamburger + footer Product column expose Leaderboard, Compare, and Shows on every marketing page (`/`, `/methodology`, `/privacy`, `/terms`). Single source: `src/config/marketingNav.ts` `EXPLORE_LINKS`. [Feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md).

**Reddit Ads pixel and Conversions API live** — consent-gated `PageVisit` + pixel `SignUp`; server `SIGN_UP` since 2026-09-24 (test mode off the same day). Click id rides the magic link. A listed `internal_accounts` row skips the pixel and the server SignUp (2026-09-25). Two-week read kept the €10/day test budget (2026-10-08). See [[concepts/reddit-ads-conversion-tracking]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md).

**Cumulative performance charts shipped** — profile vs S&P, `/compare` head-to-head, Play animation, watermarked PNG/JPEG/MP4 export. See [[concepts/cumulative-performance-charts]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md).

**Subscription entitlement SSOT live on production** — profile tier is the only app entitlement source; Stripe is billing-only with webhook + reconcile self-heal; combined-performance base-table loophole closed; H9 client self-upgrade closed. See [[concepts/subscription-entitlement-ssot]] and [feature doc](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md).

**LinkedIn on profiles:** Supabase `finfluencers.linkedin_url` is populated from [[projects/gor_dagster]] trusted provenance only. See [[concepts/linkedin-enrichment]].

**Outreach chart render (2026-08-11):** headless Playwright harness reuses the cumulative-chart stack for Notion-bound MP4s (`scripts/render-outreach-charts.mjs`, `src/lib/outreach*.ts`). Orchestration and Notion write-back live in [[projects/gor_dagster]] — [[concepts/linkedin-outreach]]. No LinkedIn send from the app.

**CNBC IPO scoreboard live** at **`/cnbc-ipo`** — public, no auth; Supabase RPC `get_ipo_scoreboard_page`.

**Public conversion funnel shipped (2026-07-10).** Anonymous visitors browse `/leaderboard` and `/shows` without login; profile depth gated behind free Spectator signup; Trader for non-featured finfluencer data. Persistent marketing chrome to those routes shipped **2026-08-17**.

## Access model (Option 2 3-tier gating, 2026-10-30)

| Surface | Anonymous (`anon`) | Spectator (Free Account) | Trader (€19/mo) |
|---------|--------------------|--------------------------|-----------------|
| `/cnbc-ipo` (IPO scoreboard) | Public | Public | Public |
| Leaderboards (`/leaderboard`, `/leaderboard/sectors`, `/shows`) | Public: 3 `is_free_tier` finfluencers unmasked; remaining 81 masked (`NULL` before `ORDER BY`/`ROW_NUMBER()`) behind free-account CTA | **Full unmasked** across all 84 finfluencers & 14 shows (`1W`–`1Y`) | **Full unmasked** |
| `/upgrade` (Pricing) | Public | Unchanged | Unchanged |
| `/compare` | Prefilled Cramer vs Brown; locked subjects redirect to `/signup` | **Full picker** across all 84 finfluencers | **Full picker** |
| Finfluencer profile (KPIs, cumulative chart, sector widget, ticker lookup) | 3 `is_free_tier` unmasked; remaining 81 gated behind free-account signup | **Full aggregate track record** across all 84 finfluencers | **Full aggregate track record** |
| Show profile | Teaser + free-account gate | **Full** | **Full** |
| `/signals` & profile `<FreshScoredPicksSection />` (sessions 1–10) | Public `/signals`: 3 featured finfluencers' fresh picks visible with `stars`/`explanation_text`/`context_line` masked (`NULL`) behind `"Create a free account to see score"` | **3 featured finfluencers** unmasked with `1★–5★` stars & `"Why N stars?"`; remaining 81 gated behind Trader CTA | **All 84 finfluencers unmasked** |
| Historical individual picks (> 10 sessions old) | Revoked (aggregates only) | Revoked (aggregates only) | Revoked (aggregates only) |

Under **Option 2 (2026-10-30)**, signing up for a free **Spectator** account unlocks 100% of historical aggregate track records across all 84 finfluencers and 14 shows plus `1★–5★` Prediction Scores for the 3 featured finfluencers, while **Trader (€19/mo)** unlocks fresh `1★–5★` scored picks and exits across all 84 finfluencers ([[concepts/prediction-scores]], [[concepts/subscription-entitlement-ssot]]).

## Tech stack

- **Frontend:** React 18, Vite, Tailwind, shadcn/ui, TanStack Query, React Router
- **Hosting:** Vercel (SPA + `api/` serverless for OG meta, sitemap, and the field-RUM sink `api/rum`)
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
| [prediction-scores](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/prediction-scores.md) | `1★–5★` 6M Prediction Scores (`blend_w50_v1`), 10-session fresh window (`get_fresh_scored_picks`), Option 2 3-tier gating (`Guest` → `Spectator` → `Trader`), evidence-backed `"Why N stars?"` explanations, and `context_line` sample-size rules (**2026-10-30**) |
| [cumulative-performance-comparison-charts](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md) | Cumulative % charts, `/compare`, Play, export (**2026-08-04**) |
| [reddit-pixel-tracking](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md) | Reddit pixel, consent gate, SignUp conversion (**2026-08-17**); listed accounts skip SignUp (**2026-09-25**) |
| [landing-tour-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-tour-improvements.md) | 50/50 first-visit tour, close and abandon events, sign-up arm tag, internal-account skip (**2026-10-20**) |
| [mobile-experience-uplift](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-experience-uplift.md) | Document scroll below `md`, one-table lists, shorter landing, first-visit tour (**2026-10-20**) |
| [rum-off-ga4](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/rum-off-ga4.md) | Web vitals to `/api/rum` and Supabase, not GA4 (**2026-10-20**) |
| [session-replay-analytics](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md) | Clarity replay, consent gate, masking, funnel events (**2026-08-18**) |
| [conversion-measurement-plan](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/conversion-measurement-plan.md) | Consent, `purchase`, key events imported to Ads as secondary; `sign_up` is the only primary (**2026-10-20**) |
| [public-navigation-discoverability](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md) | Marketing Explore nav to Leaderboard / Compare / Shows (**2026-08-17**) |
| [feedback-decline-with-note](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md) | Declined status, admin note, notify intent marker (**2026-08-22**) |
| [core-web-vitals-mobile](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-mobile.md) | Mobile LCP/INP delivery: splitting, fonts, cache, idle third-party (**2026-08-22**; lab-vs-field amended **2026-09-08**) |
| [core-web-vitals-field](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-field.md) | First-party field RUM, DATA_READY, field-decides-done (**2026-09-08**) |
| [profile-leaderboard-script-split](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/profile-leaderboard-script-split.md) | Show data-ready, profile script inventory, Supabase stand-ins; body split not shipped (**2026-09-28**) |
| [google-ads-creative-assets](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/google-ads-creative-assets.md) | PMax images + video from the shipped encoder; banner / overlay / outro (**2026-09-04**) |
| [ads-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ads-improvements.md) | Search RSA studio stills; overlay carve-out is PMax-only; stills in `docs/ops/google-ads-search-images/` (**2026-09-27**) |
| [investor-positioning-landing-ads](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/investor-positioning-landing-ads.md) | Homepage hold-period promise; 6-month teasers; ads checklist, no ads API (**2026-09-27**) |
| [mobile-signup-drop](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-signup-drop.md) | Why mobile sign-ups fell after `29b315f` (Search mix, not the deploy); `paywall_view` / `paywall_cta_click`; new-users-per-day query; deploy annotations in `docs/ops/production-deploys.md` |
| [search-qs-landing-page](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/search-qs-landing-page.md) | Leaderboard heading and supporting line; Search ad-group checklist; Quality Score is evidence (**2026-10-14**) |
| [customer-list-ad-audiences](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/customer-list-ad-audiences.md) | Registered-user Customer Match; Monday CSV refresh; Reddit parked (**2026-09-22**) |
| [finfluencer-ticker-pick-lookup](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/finfluencer-ticker-pick-lookup.md) | Finfluencer ticker lookup + ranked-list RPC; two box-plot charts retired (**2026-09-05**) |
| [post-signup-onboarding](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/post-signup-onboarding.md) | Auth-callback onboarding detour; skippable wizard; pending dest (**2026-09-09**) |
| [onboarding-questionnaire-v2](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-questionnaire-v2.md) | One research question per screen; channel, follows, names, holding period (**2026-09-28**) |
| [onboarding-skip-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-skip-improvements.md) | Inline validation; Skip stays; loading masks are not hit targets; completion gate not met (**2026-10-06**) |
| [user-default-holding-period](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/user-default-holding-period.md) | System start 6 months; Settings pin; URL omit uses system default (**2026-09-16**) |
| [sector-leaderboard-wide-name-filter](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/sector-leaderboard-wide-name-filter.md) | Name filter in the sector table header at every width; top bar still opens `/leaderboard` (**2026-09-24**) |
| [ui-accessibility-and-test-reliability](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ui-accessibility-and-test-reliability.md) | Button fill vs alpha colours; contrast enforced; phone name wrap (**2026-09-25**) |
| [daily-marks-plan](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md) | Parked: true daily portfolio marks (future) |
| [subscription-entitlement-ssot](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) | Profile SSOT, reconcile, RLS close (**2026-07-27**) |
| [landing-conversion-improvements](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) | Public funnel, SEO, anon RPC pattern (**2026-07-10**) |
| [feedback-system](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) | Living `/feedback` system: schema, RLS, notify-moderators, admin panel (**2026-06-02**, declined **2026-08-22**) |

Cross-subdomain auth: [auth-sharing-landing-app.md](file:///Users/andreskull/finfluencer-tracker/docs/auth-sharing-landing-app.md).

## Completed features

| Date | Feature | Permanent doc |
|------|---------|---------------|
| 2026-10-30 | Prediction Scores for Fresh Stock Picks & Option 2 3-tier gating (`prediction-scores`) | [prediction-scores.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/prediction-scores.md) |
| 2026-10-20 | Landing tour improvements | [landing-tour-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-tour-improvements.md) |
| 2026-10-20 | Conversion measurement (`sign_up` only primary) | [conversion-measurement-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/conversion-measurement-plan.md) |
| 2026-10-20 | RUM off GA4 | [rum-off-ga4.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/rum-off-ga4.md) |
| 2026-10-20 | Mobile experience uplift | [mobile-experience-uplift.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-experience-uplift.md) |
| 2026-10-19 | Signed-out QA findings and data integrity (`fix-findings-0810`) | [signed-out-qa-findings-fix.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/signed-out-qa-findings-fix.md) |
| 2026-10-16 | Mobile sign-up drop | [mobile-signup-drop.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/mobile-signup-drop.md) |
| 2026-10-14 | Search QS landing page | [search-qs-landing-page.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/search-qs-landing-page.md) |
| 2026-10-06 | Onboarding skip improvements | [onboarding-skip-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-skip-improvements.md) |
| 2026-09-30 | Supabase ledger baseline | [supabase-ledger-baseline.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/supabase-ledger-baseline.md) |
| 2026-09-29 | Information Ratio on the leaderboard, profile cards, Methodology, and the Trader email | Cross-repo: [information-ratio.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/information-ratio.md) |
| 2026-09-28 | Profile and leaderboard script split | [profile-leaderboard-script-split.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/profile-leaderboard-script-split.md) |
| 2026-09-28 | Onboarding questionnaire v2 | [onboarding-questionnaire-v2.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/onboarding-questionnaire-v2.md) |
| 2026-09-28 | Finfluencer, show, and ticker profile pages (show/leaderboard/panelist/ticker pages, M1–M14; FR-1.11 cancelled before build) | Cross-repo: [finfluencer-and-show-profiles.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/finfluencer-and-show-profiles.md) |
| 2026-09-27 | Ads improvements (Search RSA image assets) | [ads-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ads-improvements.md) |
| 2026-09-27 | Investor positioning — landing and ads | [investor-positioning-landing-ads.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/investor-positioning-landing-ads.md) |
| 2026-09-25 | UI accessibility and test reliability | [ui-accessibility-and-test-reliability.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ui-accessibility-and-test-reliability.md) |
| 2026-09-24 | Sector leaderboard wide-screen name filter | [sector-leaderboard-wide-name-filter.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/sector-leaderboard-wide-name-filter.md) |
| 2026-09-16 | User-default holding period | [user-default-holding-period.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/user-default-holding-period.md) |
| 2026-09-09 | Post-signup onboarding | [post-signup-onboarding.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/post-signup-onboarding.md) |
| 2026-09-08 | Field Core Web Vitals and data-ready | [core-web-vitals-field.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-field.md) |
| 2026-09-05 | Finfluencer × ticker pick lookup | [finfluencer-ticker-pick-lookup.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/finfluencer-ticker-pick-lookup.md) |
| 2026-09-04 | Google Ads creative assets | [google-ads-creative-assets.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/google-ads-creative-assets.md) |
| 2026-08-22 | Mobile Core Web Vitals | [core-web-vitals-mobile.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-mobile.md) |
| 2026-08-22 | Feedback: declined status with admin note | [feedback-decline-with-note.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/feedback-decline-with-note.md) |
| 2026-08-18 | Session replay & behavioural analytics | [session-replay-analytics.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/session-replay-analytics.md) |
| 2026-08-17 | Public navigation discoverability | [public-navigation-discoverability.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/public-navigation-discoverability.md) |
| 2026-08-17 | Reddit pixel tracking | [reddit-pixel-tracking.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md) |
| 2026-10-09 | Reddit Conversions API (same doc) | [reddit-pixel-tracking.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/reddit-pixel-tracking.md) |
| 2026-08-04 | Cumulative performance comparison charts | [cumulative-performance-comparison-charts.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/cumulative-performance-comparison-charts.md) |
| 2026-07-27 | Subscription entitlement SSOT | [subscription-entitlement-ssot.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/subscription-entitlement-ssot.md) |
| 2026-07-23 | LinkedIn URL on finfluencer profiles (trusted sync) | Cross-repo: [linkedin-enrichment.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/linkedin-enrichment.md) |
| 2026-07-11 | CNBC IPO scoreboard (`/cnbc-ipo`, SPCX v1) | Cross-repo: [cnbc-ipo-scoreboard.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/cnbc-ipo-scoreboard.md) |
| 2026-07-10 | Landing page & conversion funnel improvements | [landing-conversion-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/landing-conversion-improvements.md) |
| 2026-06-02 | Custom feedback & public roadmap | [feedback-system.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/feedback-system.md) |

## Key architecture decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-10-30 | Individual picks & `1★–5★` Prediction Scores are shown strictly for the first 10 US trading sessions after entry (`get_fresh_scored_picks`); raw client `SELECT` on `signals`, `signal_performance`, `signal_current_performance`, and `pick_scores` is revoked | Prevents bulk scraping of historical picks while delivering real-time actionable signals during the fresh window; on session 11, picks roll off into aggregate track records |
| 2026-10-30 | Option 2 3-tier gating (`Guest` → `Spectator` → `Trader`) unlocks 100% of historical aggregate track records for free signed-in Spectators while gating non-featured fresh `1★–5★` scored picks to Trader | Gives free account signups high immediate value (all 84 finfluencers' aggregate track records + 3 featured finfluencers' `1★–5★` scores) while keeping daily `1★–5★` scored picks across all 84 finfluencers as the core Trader upgrade driver |
| 2026-10-30 | Public `/signals` is accessible to anonymous visitors (`anon`) with `stars`, `explanation_text`, and `context_line` masked to `NULL` in SQL | Lets anonymous visitors browse real fresh stock picks from the 3 featured finfluencers while requiring a free Spectator signup to reveal the `1★–5★` Prediction Score and `"Why N stars?"` explanation |
| 2026-10-30 | `leaderboard` and `get_sector_leaderboard` mask locked rows (`NULL`) **before** `ORDER BY` and `ROW_NUMBER()` | Sorting on raw unmasked alpha/IR before masking leaked the relative rank order of locked finfluencers to anonymous visitors |
| 2026-10-30 | Evidence-backed `"Why N stars?"` explanations enforce the `7 of 9` years stability rule and `"heavily traded stocks"` wording; `context_line` requires `>= 3` prior picks to quote an average | Prevents small-sample noise from appearing in human-readable explanations (`1–2` prior picks quote count only; `0` emits empty string) |
| 2026-10-20 | Field RUM (`web_vitals`, DATA_READY) goes to `/api/rum` and Supabase `web_vitals_events`, never to GA4 | gtag batches hits and a frozen iOS page delivers them hours later inside the old GA4 session; no gtag flag makes an event non-session-extending, so an idle guard at report time cannot fix it |
| 2026-10-20 | A migration the client code depends on goes to production before the code is promoted | Until the table exists, production reports fail to save and are lost; visitors see nothing either way |
| 2026-10-20 | Production e2e specs intercept `/api/rum`, and the e2e fixture sets `ft_internal_v1` | Each unintercepted run inserted real rows (116 deleted by hand), and each fresh browser counted as new users in GA4 |
| 2026-10-20 | Below `md` the document scrolls and `--app-topbar-h` positions sticky chrome; from `md` the inner `main` scrolls | Mobile browser chrome never retracted when the document height equalled the viewport; `env(safe-area-inset-*)` is 0 on iOS without `viewport-fit=cover` |
| 2026-10-20 | List pages are one `<table>` with CSS column visibility and `table-fixed md:table-auto` | `table-layout: auto` lets a long name push the metric column off-screen in WebKit. Check a new page's metric count and label length against 320 px before using `SortPillRow` |
| 2026-10-20 | A page reading `useUserDefaultHorizon().defaultHorizon` needs the `horizonTouchedRef` plus sync-effect pattern | The value resolves after first render; a bare `useState` freezes on the system default and re-clicking the saved radio is a silent no-op |
| 2026-10-20 | The landing tour is a lazy chunk on idle, gated by a pure `shouldShowTour`, with a build-time kill switch | Zero pre-LCP bytes on the paid entry page; consent banner always wins |
| 2026-10-20 | The landing-tour arm is stored in the browser; `tour_assign` fires in both arms | There is no remote flag service. Counting `tour_view` alone hides the control group |
| 2026-10-20 | `internal_accounts` is the only test-user list; a listed account skips Ads and Reddit SignUp | The skip keys on the account, not the browser flag. GA4 `sign_up` and `signup_attribution` still record it |
| 2026-10-20 | The tour test stays at 50/50 | First read was 6 vs 7 sign-ups. The Thursday ads review reads the funnel |
| 2026-10-20 | `sign_up` stays the only Google Ads primary; `comparison_created` and `purchase` stay secondary | Last 30 days: 99 signups, 5 comparisons, 0 purchases. Promoting either would change bidding without a series to optimise |
| 2026-08-11 | Imported GA4 events are secondary; a URL rule is never primary | The old primary counted arrivals at `/signup`. Key-event status is what exposes an event to Ads |
| 2026-10-19 | Showcase snapshots are committed static fixtures guarded by build and cron, not live client RPCs | Live RPC fetches full history since 2015 (thousands of points) and costs compute on every guest load; static fixtures downsample to 180 points and prebuild fails if older than 31 days |
| 2026-10-19 | Single-paid-panelist shows (>= 80% picks) mask aggregates identically to paid individual profiles | A show dominated by one paid individual (*The Morning Filter* at 100%, *Hedge Fund Tips* at 99.5%) acts as an unmasked proxy for a paid track record |
| 2026-10-19 | Q4 pick count predicate includes holds; is_kept is isolated strictly to sector averages | Holds are genuine conviction signals and part of public mention count; filtering is_kept in raw counts caused headline vs sector alpha gaps |
| 2026-10-16 | Diagnose a sign-up drop in `auth.users` first, and split GA4 by network and country before suspecting a deploy | Blended GA4 Search hid Search Partners and a location cut that removed non-US sign-ups. The deploy was not the cause |
| 2026-10-16 | Add `paywall_view` and `paywall_cta_click`; leave `paywall_hit` unchanged | `paywall_hit` fires at gate mount (page load on a profile). Changing its meaning mid-diagnosis would have broken the comparison |
| 2026-10-16 | A code rollback cannot undo a column drop | `sharpe` was dropped after `29b315f`; plan rollbacks around database state, not the commit |
| 2026-09-30 | Migration history is a baseline pair, not 73 replays | Replaying the old files never matched production. A generated schema file plus a supplement for what `pg_dump --schema=public` omits (publication membership, `auth.users` triggers) replays to a snapshot-identical schema. Old files stay in `migrations_archive/` |
| 2026-09-30 | Replay is proved with a catalog snapshot, not by eye | `scripts/ledger-catalog-snapshot.sql` diffed against production; approved exceptions live in `scripts/ledger-snapshot-exceptions.txt` (`pg_net` extension, notification function URL). Run `npm run db:replay-check` before any push |
| 2026-09-30 | Live histories repaired, live schema untouched | `migration repair` reverted the 73 old versions and applied the two baseline versions on development and production. Schema snapshots matched before and after; backups of `schema_migrations` kept |
| 2026-09-30 | Do not install extensions the app does not use | The replay dropped `pg_graphql` in the supplement (`DROP EXTENSION IF EXISTS`) so a local stack matches production |
| 2026-09-28 | Show data-ready is the real header row, from one `isDataReadyRoute` list | A guest cannot read below `TierGate`. A second route list in `BottomNav` prefetched before that header |
| 2026-09-28 | Unused Supabase realtime and storage are build aliases | `supabase-js` constructs both inside `createClient`, so tree-shaking cannot drop them. Deleting the two alias lines restores the real clients |
| 2026-09-28 | Show profile is data late; desktop `/leaderboard` is not enough samples | No show body split and no leaderboard page change. A shell `shows` request, and a desktop field read that drops 40 ms no-geography samples, are later features |
| 2026-09-28 | The profile body split and the lazy toaster were not shipped | The body (at least 93 KB) was the cut to the 591,608 byte target. The toaster is 13 KB. Select and the Radix overlay stack stay in `app-core` |
| 2026-09-28 | The inline bootstrap preloads landing chunks only on `/` | They were on every non-ad path. A bare `import.meta.env` must not be read: Vite inlines the whole env object into `ad-shared` |
| 2026-10-06 | Do not raise onboarding completion by hiding Skip | Skip is the measured choice. A loading mask is not a hit target. The 50% bar was missed (6 of 14, 29 Sep–5 Oct) |
| 2026-09-28 | Onboarding research holding period is not the chart pin | Screen D writes `holding_period_preference`. It does not read or write `user_profiles.default_holding_period`. |
| 2026-09-28 | Onboarding `step` reuses the Tour Step GA4 dimension | One custom dimension per parameter name. Filter reports by event name. `name_count` is a custom metric. |
| 2026-09-27 | Search RSA images are not PMax harness JPEGs | Google's overlay carve-out is PMax-only. Chart chrome and in-pixel UI text fail Search as "Text or graphic overlays". |
| 2026-09-27 | No chrome-stripped Search exporter; no harness change for Search stills | That would be a new encoder contract. Search assets are studio photographs in `docs/ops/google-ads-search-images/`. |
| 2026-09-27 | Homepage and ads subject period is 6 months; signup stays open | Teasers use `DEFAULT_HORIZON`. A missing 6-month ads subject fails (`no_6m_coverage`) and does not fall back. Scalp exclusion is copy and negatives, not a funnel block. |
| 2026-09-27 | Ads account edits stay a UI checklist | No ads API client, no new campaign, no budget or bid change. Runbook: `docs/ops/investor-positioning-ads-checklist.md`. |
| 2026-09-25 | Button fill and alpha colours are separate tokens; `text-primary` uses `--positive` | `#1f812d` clears white on a button (4.67:1) and fails as small text. Alpha uses `#51ad57` / `#e36b65`. |
| 2026-09-25 | `color-contrast` stays on; HowItWorks numerals are the only exemption | They are a watermark: `text-border`, `aria-hidden`, `data-contrast-exempt`. |
| 2026-09-28 | Feedback board smoke is on demand, not in `npm run test:e2e` | Fixture inserts email moderators through Resend. One suite run per browser project was spending the daily quota. `npm run test:e2e:feedback` runs Chromium once. |
| 2026-09-25 | A full local Playwright matrix is not a release gate | Every browser at once against one server fails a different handful of waits each pass. CI uses one worker. |
| 2026-09-24 | Sector name filter visibility is a class on the sector page, not a change to `NameFilterInput` | `/leaderboard` and `/shows` keep a phone-only filter. The top bar still opens `/leaderboard`. |
| 2026-09-16 | System holding-period start is `DEFAULT_HORIZON = '6m'` on every product view | Unifies the old 1m/1y split; Settings is the only writer |
| 2026-09-16 | URL omit uses the **system** default, not the signed-in save | A guest opening a copied clean URL gets 6m, not someone else’s pin |
| 2026-09-16 | `user_profiles.default_holding_period` is nullable preference GRANT, not a `'6m'` insert default | Unset follows a future system-default change; explicit `'6m'` is a pin |
| 2026-08-28 | Onboarding has one trigger: the auth callback. The 24-hour `ProtectedRoute` gate is gone | Two triggers cannot both refuse a returning same-day sign-in and refuse to re-trap a skip |
| 2026-08-28 | Funnel counts for `sign_up` → onboarding live in GA4, not Clarity | Clarity skips credential URLs and idle-defers first init |
| 2026-08-29 | Magic-link `emailRedirectTo` carries `?redirect=`; OAuth does not | A mail client opens a new tab where `sessionStorage` is empty |
| 2026-09-05 | Unbounded `signals` aggregates go through a server-side RPC, never a raw PostgREST select | PostgREST caps at 1000 rows with no `ORDER BY`; ranked counts and “no picks” empties were silently wrong |
| 2026-09-05 | `SECURITY DEFINER` finfluencer RPCs re-apply `get_user_tier()` and grant `authenticated` only | `show_ticker_list`’s anon grant does not transfer — show profiles are unlocked, paid finfluencer picks are not |
| 2026-09-05 | Lookup stays inside `TierGate`; no guest CTA in the control | Guests are already locked; a CTA under blurred `pointer-events-none` children is unreachable |
| 2026-09-05 | Retire pipeline-owned app snapshots frontend → pipeline job → DROP | Any other order 404s live profiles or fails a pipeline write |
| 2026-09-05 | Do not drop indexes whose migration comments name a retired chart | Those indexes serve platform traffic, including `finfluencer_ticker_list` |
| 2026-09-04 | Ads image ratios live in `adsExportLayout.ts`, never in `ExportAspectRatio` | Share menu cannot leak 1.91:1 / 4:5 |
| 2026-09-04 | Banner / outro are optional encoder callbacks defaulting off | Outreach and on-site export stay byte-identical (per-frame invariance harness) |
| 2026-09-04 | Ads featured subject ranked on scored picks at `1y`, not alpha | Alpha selected a subject whose drawn line trails the index; the claim is coverage |
| 2026-08-22 | Pre-LCP JS budget is the decoded sum of scripts completed before LCP on `/`, not the largest chunk | Once routes are lazy there is no single entry file; a largest-chunk budget is gameable |
| 2026-08-22 | Landing stays eager; Suspense fallbacks reserve space and render nothing visible | A lazy boundary in front of the hero, or a flashing spinner, defeats the LCP work |
| 2026-08-22 | Third-party deferral injects the `<script>` on idle; gates and shims stay sync | Deferral must not lose, duplicate, or reorder conversions, or weaken consent |
| 2026-08-22 | Profile headers paint from the slug / first row, not after every related fetch | A skeleton with zero text nodes is an LCP tax; SSR/prefetch is a later architecture change |
| 2026-09-08 | Field decides CWV done; lab decides whether a change helped | First-party p75 is the done gate; lab `slow4g` TTFB can be an edge HIT |
| 2026-09-08 | Time-to-populated-rows is `DATA_READY`, not a Core Web Vital | LCP/CLS can be “good” on a skeleton; `/` teaser ≤ 2.0 s, `/leaderboard` ≤ 1.5 s |
| 2026-08-22 | Lab measurement is the CWV gate; field data is a 28-day confirmation | **Amended 2026-09-08** — field is the done gate. CrUX silence is still expected; it is not a pass. |
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
| 2026-08-12 | Reddit `SignUp` rides `maybeTrackSignUp`; `conversionId = user.id` | One dedupe path with Google; Pixel and CAPI collapse on that id |
| 2026-08-12 | Advanced Matching: privacy clause now, hashed email not sent on the pixel | CAPI hashes the email instead (2026-10-09). `rdt('init')` still gets no match keys |
| 2026-10-09 | Reddit click id rides the magic link as `ml_*` | The mail app does not share `localStorage` with the browser that loaded the ad |
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
- **Prediction Scores (`public.pick_scores` & `get_fresh_scored_picks`):** Synced from BigQuery `dagster_sync.mat_pick_scores`; direct `SELECT` revoked on `signals`, `signal_performance`, `signal_current_performance`, and `pick_scores` for `anon`/`authenticated`; `get_fresh_scored_picks(p_finfluencer_id, p_direction, p_min_stars, p_limit, p_offset)` serves fresh picks and exits within 10 US trading sessions (`sessions 1–10`) with server-side Option 2 tier masking ([[concepts/prediction-scores]])
- **Holding-period preference:** `user_profiles.default_holding_period` (nullable text, CHECK `1w|1m|3m|6m|1y`); `authenticated` UPDATE on that column only; `NULL` = follow system 6 months ([[concepts/user-default-holding-period]])
- **`finfluencer_combined_performance_public`** + **`leaderboard`** — security-definer masked views; under Option 2 (`20261030120000`), `anon` sees 3 `is_free_tier` rows unmasked and remaining 81 nulled **before** `ORDER BY`/`ROW_NUMBER()`, while `authenticated` (`spectator`/`trader`) sees all 84 unmasked; base table not readable by anon
- **`benchmark_daily_prices`** — SPY daily adj_close (synced from BigQuery `PriceHistory`)
- **`get_cumulative_performance_series(...)`** — security-definer RPC; entitlement mirrors TierGate / `is_free_tier` ([[concepts/cumulative-performance-charts]])
- **`get_ipo_scoreboard_page(p_ticker)`** RPC — CNBC IPO scoreboard payload
- **`landing_stats()`** RPC — landing stats bar
- **`finfluencer_ticker_list(...)`** — definer + `get_user_tier()` gate; `authenticated` only; ranked mode and `p_ticker` lookup ([[concepts/finfluencer-ticker-pick-lookup]])
- **`best_worst_calls` / `instrument_ticker_summary`** — Featured Calls and instrument-header α; latter is INVOKER so RLS still hides paid picks
- **`signal_performance UNIQUE (signal_id, requested_horizon)`** — implicit unique index replaced `idx_sigperf_signal_horizon`; covering index stays
- **`show_ticker_list`** — same variant sign filter as the finfluencer RPC (`>= 0` / `<= 0`); still granted to `anon` + `authenticated`
- **`show_summary_stats()` / `show_distinct_ticker_counts()`** — guest `/shows` columns; keep-list (do not drop with retired chart RPCs)
- **`signup_attribution`** — one row per user; authenticated insert of own row only; no client select. First-touch flags, not the click ids
- **`feedback_posts`** — statuses include `declined`; `admin_note` + `notify_requested_at`; vote RLS allowlist `under_review`/`planned`; never rewrite `handle_feedback_notification()` ([[concepts/feedback-roadmap]])
- Promotion: Preview → development Supabase + Stripe test; production Vercel → production Supabase + live Stripe. Edge function `notify-moderators` deploys **twice**.

## MVP and planning docs (cross-repo)

Shipped MVP scope: [`gor_dagster/docs/MVP_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/MVP_MASTER_PLAN.md). Post-MVP backlog: [`INCR_01_MASTER_PLAN.md`](file:///Users/andreskull/gor_dagster/docs/INCR_01_MASTER_PLAN.md). Operations: [`finfluencers-app-runbook.md`](file:///Users/andreskull/gor_dagster/docs/operations/finfluencers-app-runbook.md). Growth: [[synthesis/gtm-growth]]. Full table: [[products/finfluencer-trade]] § Planning and strategy.

## Deferred / out of scope

- Promoting `purchase` to primary, once there is a real series of payments. Client-side `purchase` still misses renewals ([conversion-measurement-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/conversion-measurement-plan.md))
- Terminal product SKU / Terminal downward-reconcile product map
- Onboarding completion stayed under 50% after the skip-path work (6 of 14, 29 Sep–5 Oct 2026). Skip stays. A later lift is a new product decision — [[concepts/post-signup-onboarding]]
- Magic-link / OTP deliverability (Gmail Spam on a young domain; `rua=` DMARC still open; not an app defect)
- Email capture / newsletter on landing
- Embedded Stripe Payment Element migration
- IPO scoreboard access model changes
- Show-profile ticker lookup; per-pick history under the summary; `?ticker=` deep link; TopBar ticker search ([[concepts/finfluencer-ticker-pick-lookup]])
- True daily portfolio marks (parked — [daily-marks-plan.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/daily-marks-plan.md)); show↔show compare; non-S&P benchmarks
- Search RSA women square still Pending at wrapup (2026-09-27); glance on the next `ads-performance-morning-review` A9 pass — do not replace unless Disapproved ([ads-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ads-improvements.md))
- 6-month Performance Max stills were not uploaded. The ops checklist allows that upload inside the existing asset group ([investor-positioning-landing-ads.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/investor-positioning-landing-ads.md))
- Pixel Advanced Matching (hashed email on `rdt('init')`). CAPI already hashes it. Revisit only if SignUp match quality stays unscored ([[concepts/reddit-ads-conversion-tracking]])
- Official Clarity↔GA4 OAuth dashboard link (playback URLs in GA4); campaign tags already on recordings ([[concepts/session-replay-analytics]])
- Feedback note history / threading / voter-notify; reconstructing deleted submitter accounts; declined-last ordering must move server-side if the board paginates ([[concepts/feedback-roadmap]])
- Profile body still downloads before the first card (at least 93 KB). The 591,608 byte target was not met. Stand-ins are on `development`, not production ([profile-leaderboard-script-split.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/profile-leaderboard-script-split.md))
- Show profile is data late. A shell `shows` request is a later feature. No hard `DATA_READY` baseline yet
- Desktop `/leaderboard` needs a field read that excludes 40 ms no-geography samples before any page change
- `/show/:slug` still 0.04 s over 2.5 s lab LCP — would need route-level prefetch / SSR ([[concepts/core-web-vitals-mobile]])
- Four Search Console CWV URLs are MkDocs — a `gor-blog` follow-up if the CrUX *group* is to go Good
- Search Console field-data confirmation ~**2026-09-19**; do not resubmit Validate Fix before then

## Wiki sync

Vault indexes **WIKI.md** and all of **`docs/`** except **`docs/features/`**. Run **`wiki sync finfluencer-tracker`** after substantive doc updates.

## Related pages

- [[products/finfluencer-trade]]
- [[projects/gor_dagster]]
- [[projects/gor-blog]]
- [[concepts/prediction-scores]]
- [[concepts/user-default-holding-period]]
- [[concepts/post-signup-onboarding]]
- [[concepts/core-web-vitals-field]]
- [[concepts/core-web-vitals-mobile]]
- [[decisions/field-authoritative-cwv-2026-09]]
- [[concepts/feedback-roadmap]]
- [[concepts/session-replay-analytics]]
- [[concepts/reddit-ads-conversion-tracking]]
- [[concepts/google-ads-conversion-tracking]]
- [[concepts/google-ads-creative-assets]]
- [investor-positioning-landing-ads.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/investor-positioning-landing-ads.md)
- [ads-improvements.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/ads-improvements.md)
- [[concepts/finfluencer-ticker-pick-lookup]]
- [[concepts/cumulative-performance-charts]]
- [[concepts/subscription-entitlement-ssot]]
- [[concepts/signal-performance]]
- [[concepts/linkedin-enrichment]]
- [[concepts/linkedin-outreach]]
- [[concepts/signal-source-quote]]
- [[decisions/ga4-instrumentation-registration-2026-09]]
- [[entities/bigquery]]
