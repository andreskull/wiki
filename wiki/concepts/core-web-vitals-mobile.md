---
type: concept
title: "Mobile Core Web Vitals"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-08-22
updated: 2026-09-08
tags: [lcp, inp, performance, vercel, vite, search-console, ga4, bigquery, analytics]
---

# Mobile Core Web Vitals

## Definition

Delivery and loading changes that brought mobile Largest Contentful Paint on the Vite SPA under the Good threshold of **2.5 s**, and stopped third-party loaders competing with first paint (the INP lever), without changing the rendering architecture. The product stays a client-rendered SPA on Vercel. SSR and build-time prerendering were not opened.

The 8-URL Search Console group was mixed origin: four Vite SPA URLs (`/`, `/show/:slug`, `/leaderboard`, `/privacy`) and four MkDocs `/blog*` URLs proxied via `vercel.json`. This work can only move the SPA half. Blog delivery is a [[projects/gor-blog]] question.

**Amended 2026-09-08:** lab is no longer the done gate. **Field decides done; lab decides whether a change helped** — [[concepts/core-web-vitals-field]], [[decisions/field-authoritative-cwv-2026-09]]. CrUX / Search Console remains a blended group on a ~28-day cycle. The > 4 s LCP bucket passed **2026-08-26**. Do not click Validate Fix on LCP > 2.5 s.

## Relevance

Paid mobile traffic lands on `/`, leaderboards, show pages, and finfluencer profiles. A 4 s blank screen is a paid click spent on nothing, and a ranking signal for a property whose organic strategy depends on the blog and directory feeding the app.

Production lab (slow4g, Pixel 5, cold) after PR #68 + PR #70: `/` **2.03 s** (was 3.74 s); `/finfluencer/jim-cramer` **2.21 s**; `/show/mad-money-w-jim-cramer` **2.54 s** (0.04 s over target). Other public SPA routes ≤ 2.50 s.

## Which projects use it

- [[projects/finfluencer-tracker]] — shipped **2026-08-22**
- [[projects/gor-blog]] — four of the eight CrUX URLs; not fixed here

## Standing rules

- Every lab number comes from `npm run measure:cwv` (`scripts/measure-cwv.mjs`) under a recorded throttle. A number without conditions is not a result.
- Pre-LCP JS budget is the **decoded sum of scripts completed before LCP on a cold `/`**, ceiling **700 KB** in `src/config/performanceBudget.ts`. Not "the largest chunk".
- `Landing` stays eager. A Suspense boundary in front of the hero defeats the feature.
- Suspense fallbacks reserve space and render nothing visible. Chart fallbacks do not invent headings.
- Third-party deferral injects the `<script>` on idle; consent gates and command shims stay synchronous ([[concepts/session-replay-analytics]], [[concepts/reddit-ads-conversion-tracking]], [[concepts/google-ads-conversion-tracking]]).
- A skeleton with zero text nodes is an LCP tax. Profile headers paint from the slug / first row; SSR/prefetch is a later architecture change.
- Never `vercel deploy` a branch. Verify a preview by `deploymentId` and commit SHA, not by a Ready badge.
- Do not resubmit Search Console Validate Fix on LCP > 2.5 s. First-party field p75 is the done gate ([[concepts/core-web-vitals-field]]).

## Field verification instrumentation (GA4 + BigQuery)

Field confirmation depends on GA4 segmenting `web_vitals` by `browser_engine`, `device_class`,
`route_pattern`. **2026-09-02:** parameters arrived populated but were never registered as GA4
custom definitions — Explore read `(not set)`. Registration is forward-only; the Aug 31 → Sep 2
window was discarded. Remediated the same day (10 dimensions + `metric_value`; BigQuery export
to `gurus-on-record`). See [[decisions/ga4-instrumentation-registration-2026-09]] and
[[entities/bigquery]]. The field wrapup is [[concepts/core-web-vitals-field]].

## Related concepts / sources

- Permanent doc: [core-web-vitals-mobile.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-mobile.md)
- Field follow-up: [[concepts/core-web-vitals-field]]
- Ops log: [core-web-vitals.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/core-web-vitals.md)
- BigQuery export destination: [[entities/bigquery]]
- Clarity idle injection: [[concepts/session-replay-analytics]]
- Reddit / GA4 gates unchanged: [[concepts/reddit-ads-conversion-tracking]], [[concepts/google-ads-conversion-tracking]]

## Related pages

- [[concepts/core-web-vitals-field]]
- [[decisions/field-authoritative-cwv-2026-09]]
- [[projects/finfluencer-tracker]]
- [[projects/gor-blog]]
- [[products/finfluencer-trade]]
