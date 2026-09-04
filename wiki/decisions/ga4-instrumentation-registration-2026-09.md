---
type: decision
title: "Register GA4 custom dimensions and link BigQuery export for Web Vitals telemetry"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-02
updated: 2026-09-02
tags: [finfluencer-tracker, ga4, bigquery, analytics, core-web-vitals, decision]
---

# Register GA4 custom dimensions and link BigQuery export for Web Vitals telemetry

## Context

`T1.1a` (read the field evidence for the mobile Core Web Vitals fix, [[concepts/core-web-vitals-mobile]]) depends on segmenting the `web_vitals` GA4 event by `browser_engine`, `device_class`, `route_pattern`, etc. — central to the Blink-vs-WebKit hypotheses (H1/H2) in the feature's design doc.

2026-09-02 read-only health check found the instrumentation pipe healthy — all 19 event parameters populated on live traffic, confirmed in Realtime and DebugView — but **GA4 Admin → Custom definitions showed 0 of 0 registered**. An unregistered event parameter is stored but not queryable in Explore/Reports (reads `(not set)`), and registration is **forward-only** — it does not backfill. This was a `T0.3`/`T0.5` gap: those tasks' DoD ("events arrive with every param populated") was true but incomplete — they never registered the params as GA4 custom definitions. The `T1.0b` deploy-free measurement window had been collecting unsegmentable data since Aug 31.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| **Register only the 4 dimensions needed for H1/H2** (`browser_engine`, `browser_engine_version`, `device_class`, `route_pattern`) | Minimal change | Leaves `navigation_type`, INP attribution, and build-verification (`debug_deploy_sha`) unsegmentable; a second gap discovered later would restart the clock again |
| **Register all ~10 analysis-relevant dimensions + `metric_value` metric now (chosen)** | One remediation, not several; well under GA4's 50-dimension cap; `metric_name` is non-negotiable (values are meaningless without it) | None material |
| **Skip BigQuery export** | Less account-settings surface | No fallback beyond GA4's UI limits; no raw-event/percentile access for `TZ.1` |
| **Enable BigQuery export alongside registration (chosen)** | Robust path for `TZ.1`; no dimension cap; real percentile control; also forward-only, so no reason to delay | Requires picking a GCP project + data location |

GA4 built-ins (`ga_session_id`, `page_location`, etc.) and `batch_ordering_id`/`batch_page_id` were deliberately excluded from registration — not analysis-relevant.

## Decision made

**2026-09-02.** Registered as **event-scoped custom dimensions**: `browser_engine`, `browser_engine_version`, `device_class`, `route_pattern`, `navigation_type`, `metric_name`, `metric_rating`, `attribution_target`, `attribution_breakdown`, `debug_deploy_sha` (10 total). Registered **`metric_value`** as a custom metric, Event scope, **Standard unit** (not Milliseconds — values mix ms for LCP/INP/TTFB/DATA_READY with the unitless CLS score; `T1.1a` must filter by `metric_name` before aggregating).

Linked **BigQuery export** to GCP project **`gurus-on-record`** (user's choice — the only account-settings input that couldn't be made unilaterally): Daily export, event + user data, full event set, no exclusions, **data location: United States (us)**, GA4's default.

**BigQuery region — closed same day, not revisited:** kept US multi-region. Rationale: this is site operational/performance telemetry, not user data — consent-gated, pseudonymous, EEA decliners already excluded from analytics consent. No EU-residency requirement applies.

## Consequences

- Segmentable-data clock restarts from 2026-09-02; Aug 31 → Sep 2 data stays unsegmentable (both in GA4 UI and BigQuery — registration/export are forward-only).
- `T1.1a` (read the evidence) earliest real start slips to **~2026-09-05/06** (3 clean days), from the originally planned ~2026-09-03.
- Registration and BigQuery linking are account-settings changes, not code deploys — FR-5's deploy freeze is untouched, and `development`'s ~14 commits ahead of `main` stay held regardless.
- BigQuery dataset `analytics_485294334` populates with `events_YYYYMMDD` (and `events_intraday_YYYYMMDD`) tables once the first daily export runs (~2026-09-04 sanity check).

## Affected projects/repos

- [[projects/finfluencer-tracker]]
- [[concepts/core-web-vitals-mobile]]

## Related pages

- [[projects/finfluencer-tracker]]
- [[concepts/core-web-vitals-mobile]]
- [[entities/bigquery]]
- [[concepts/google-ads-conversion-tracking]]
