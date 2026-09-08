---
type: concept
title: "Field Core Web Vitals and data-ready"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-08
updated: 2026-09-08
tags: [lcp, inp, data-ready, ga4, bigquery, performance, field-rum]
---

# Field Core Web Vitals and data-ready

## Definition

First-party field RUM on finfluencers.trade: GA4 `web_vitals` events (LCP, INP, CLS, TTFB, plus `DATA_READY`) segmented by rendering engine, device class, and route, exported to BigQuery. **Field decides done; lab decides whether a change helped.**

`DATA_READY` is time from navigation (or SPA route entry) until real leaderboard/teaser rows exist. Core Web Vitals cannot see it: LCP and CLS can be “good” on a skeleton.

## Relevance

Paid traffic lands on `/`. A 2 s hero paint with a 4 s empty teaser is a paid click spent on nothing. CrUX scores a blended URL group (SPA + `/blog/*`) and lags ~28 days. Clarity gives a score without engine or route. This pipe is the only per-route, per-engine p75.

Field window on SHA `a38e360` (2026-09-05 → 09-08): `/` LCP met ≤ 2.5 s; `/` teaser DATA_READY missed (3.83–4.03 s vs 2.0 s — `landing_stats`); `/leaderboard` DATA_READY met on Blink (1.24 s). Lab TTFB 61 ms did not appear in the field (701 ms Blink `/`).

## Which projects use it

- [[projects/finfluencer-tracker]] — shipped 2026-08-31 (instrumentation), wrapup 2026-09-08
- [[projects/gor-blog]] — four of the CrUX group URLs; not measured here

## Which sources discuss it

- Permanent doc: [core-web-vitals-field.md](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/core-web-vitals-field.md)
- Ops remasure index: [core-web-vitals.md](file:///Users/andreskull/finfluencer-tracker/docs/ops/core-web-vitals.md)
- August delivery (lab LCP): [[concepts/core-web-vitals-mobile]]
- GA4 registration + BQ export: [[decisions/ga4-instrumentation-registration-2026-09]]
- Lab-vs-field rule: [[decisions/field-authoritative-cwv-2026-09]]

## Related pages

- [[concepts/core-web-vitals-mobile]]
- [[projects/finfluencer-tracker]]
- [[entities/bigquery]]
- [[decisions/field-authoritative-cwv-2026-09]]
- [[products/finfluencer-trade]]
