---
type: decision
title: "Field decides Core Web Vitals done; lab decides whether a change helped"
product: finfluencer-trade
project: finfluencer-tracker
created: 2026-09-08
updated: 2026-09-08
tags: [finfluencer-tracker, core-web-vitals, lab, field, decision]
---

# Field decides Core Web Vitals done; lab decides whether a change helped

## Context

The August mobile delivery doc ([[concepts/core-web-vitals-mobile]]) treated **lab** (`measure:cwv`, Pixel 5, `slow4g`) as the completion gate and Search Console’s ~28-day CrUX window as confirmation. That was the only field scoreboard at the time.

By 2026-09-08 first-party `web_vitals` events were in BigQuery, route- and engine-segmented. The same SHA (`a38e360`) showed lab HTML TTFB **61 ms** and field `/` Blink TTFB **701 ms**. The lab number is an edge HIT after SPA `s-maxage=60`. Treating lab as “done” would have closed a visitor-visible miss.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| Keep lab as the gate | Fast, comparable, no window wait | Misses real TTFB, real `landing_stats`, real devices |
| Treat CrUX / Search Console as the gate | Official Google scoreboard | 15-URL blend (half MkDocs), ~28-day lag, no per-route LCP |
| **Field first-party p75 is done; lab is before/after (chosen)** | Per-route, per-engine, same SHA | Needs a deploy-free window and n; not a census (consent-gated) |

## Decision made

**2026-09-08.** Field first-party p75 (BigQuery `analytics_485294334`, `debug_deploy_sha` + deploy timestamp, `metric_name` before `metric_value`, mobile vs desktop separate) decides whether a CWV or data-ready target is met. Lab decides whether a specific change helped, under a recorded throttle.

The August document is amended in place. Do not click Search Console “Validate fix” on LCP > 2.5 s.

## Consequences

- A lab TTFB or LCP win that does not appear in field p75 is not a ship.
- Next production deploys start a new SHA window; they are not unfinished work on the wrapped feature.
- `/` teaser DATA_READY ≤ 2.0 s remains unmet; cheaper `landing_stats` is a later feature if that target is still wanted.

## Affected projects

- [[projects/finfluencer-tracker]]
- [[concepts/core-web-vitals-field]]
- [[concepts/core-web-vitals-mobile]]
