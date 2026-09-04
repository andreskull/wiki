---
type: concept
title: "Google Ads creative assets"
product: finfluencer-trade
project: gor_dagster
created: 2026-09-04
updated: 2026-09-04
tags: [google-ads, pmax, creatives, charts, gcs, marketing]
---

# Google Ads creative assets

## Definition

Monthly, **manual** pipeline that renders Performance Max image and video creatives from the shipped cumulative-chart encoder, then archives each run write-once in GCS. The operator uploads by hand — no Google Ads API, no Dagster schedule.

The ad claim is **accountability, not outperformance**. Featured-subject selection is coverage (scored picks at `1y`), not alpha. Copy is descriptive; the chart is the only statement of who led.

**Live 2026-09-04** — run `2026-09-04T0804Z`: V1 (featured vs S&P) and V4 (Cramer vs Josh Brown) uploaded. V2/V3 skipped when the featured subject is Cramer (degenerate / duplicate). Three banners per variant (`worth-your-time`, `curious-cramer`, `favorite-picker`).

This is **creative production**, not conversion measurement. Measurement lives in [[concepts/google-ads-conversion-tracking]].

## Relevance

Closes the PMax video/image gap so Google stops auto-generating video from leftover assets. Search RSA image assets stay out of scope (overlay policy excepts only PMax).

## Which projects use it

- [[projects/gor_dagster]] — resolve (`build_google_ads_variants.py`), write-once archive (`archive_google_ads_run.py`), runbook
- [[projects/finfluencer-tracker]] — Playwright harness, cards, banners, additive encoder options; reuses [[concepts/cumulative-performance-charts]]

## Key rules

- Horizon is `1y`, permanently. Rank on scored picks, then window length, then name.
- Ads image ratios never enter `ExportAspectRatio` — share menu cannot leak 1.91:1 / 4:5.
- Banner / outro are optional encoder callbacks defaulting off, so outreach and on-site export stay byte-identical.
- Banner copy is a fixed table, never free text, never built from a variant. Do not overstate coverage (corpus as of 2026-09-03: 82 people, 12 shows, 37,614 scored calls).
- `current` moves only on a complete run. `--mark-live` only after a real upload.
- Archive: `gs://gor-media-prod/marketing/google-ads/` (write-once).

Ops: [google-ads-asset-refresh.md](file:///Users/andreskull/gor_dagster/docs/runbooks/google-ads-asset-refresh.md)

Permanent docs: [gor_dagster](file:///Users/andreskull/gor_dagster/docs/architecture/features/google-ads-creative-assets.md), [finfluencer-tracker](file:///Users/andreskull/finfluencer-tracker/docs/architecture/features/google-ads-creative-assets.md)

## Related pages

- [[concepts/cumulative-performance-charts]]
- [[concepts/linkedin-outreach]] — sibling harness; outreach renderer has not yet adopted the captcha-safe ops session
- [[concepts/google-ads-conversion-tracking]] — measurement, not creatives
- [[entities/gcs]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[products/finfluencer-trade]]
