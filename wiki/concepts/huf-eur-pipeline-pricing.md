---
type: concept
title: "HUF/EUR pipeline pricing (rattaproff)"
product: rattaproff
project: rattaproff
created: 2026-05-12
updated: 2026-05-12
tags: [rattaproff, pricing, currency, huf, eur, woocommerce]
---

# HUF/EUR pipeline pricing (rattaproff)

## Definition

In the rattaproff repo, supplier catalogues are in **HUF**. EUR storefront prices are derived in `calculate_prices` with parameter `eur_in_huf` (HUF per one EUR). The **Google Sheet** ground truth is built using the **default** rate (`currency_config.DEFAULT_HUF_EUR_RATE`). **Per-storefront** change detection recomputes sale and normal EUR columns at `get_huf_eur_rate(domain)` inside `_gsheet_for_domain`, so optional `SITE_HUF_EUR_RATES` can diverge from the default without changing supplier loaders.

## Relevance

Prevents silent “no update” when Woo still has old EUR but the sheet looks unchanged: the HUF base matrix `_build_supplier_huf_base_df` must include every supplier (using `retailgross` or `retailgross_norm` or `0`), otherwise that supplier’s SKUs never get repriced in the per-domain dataframe and never diff against Woo.

## Which projects use it

- [[projects/rattaproff]]

## Related pages

- [huf-eur-per-site-pricing.md](file:///Users/andreskull/rattaproff/docs/architecture/huf-eur-per-site-pricing.md)
- [[products/rattaproff]]
