---
type: concept
title: "Google Sheets ground-truth sync safety"
product: rattaproff
project: rattaproff
created: 2026-07-13
updated: 2026-07-13
tags: [rattaproff, google-sheets, gcs, robot, data-integrity]
---

# Google Sheets ground-truth sync safety

## Definition

How [[projects/rattaproff]] updates the `Rattaproff Products` Google Sheet (~40k SKUs) without truncating it on partial failure, without exceeding the 10M-cell-per-file limit, and without letting the next robot run trust a mixed old/new worksheet.

## Relevance

The sheet is merchandising ground truth: published flags, pricing, categories, permalinks. A truncated or mixed sheet causes wrong WooCommerce change plans on the following run (delisted products can reappear, new products can be missed).

## How it works

**Write (end of robot run only)** — `sync_sheet_with_ground_truth` saves the intended dataframe to GCS (`gsheet` prefix), then `replace_sheet_content_with_df` writes in place:

1. `__sync_status` tab → `in_progress` (tab auto-created on first write)
2. Chunked write with `resize=False` (grid never shrinks until all chunks succeed)
3. Trim grid to exact new row count
4. `__sync_status` → `complete`
5. Up to 20 full retries on failure; marker stays `in_progress` if all fail

**Rejected approach:** staging/backup tab in the same spreadsheet — would double cell count (~4.8M → ~9.6M+) against Google's 10M file limit.

**Read** — `load_current_gsheet` verifies sync status. If `in_progress`, falls back to the GCS snapshot saved immediately before the failed write. Legacy sheets without `__sync_status` load normally with a warning.

**Same-day shops:** Woo sync uses in-memory `gsheet_new_df` before the sheet write; partial sheet state does not affect the current run's storefront updates.

## Projects using it

- [[projects/rattaproff]] — `gsheet.py`, `sheet_utils.py`, `process.py`

## Sources

- [gsheet-ground-truth-sync.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-ground-truth-sync.md)
- June 2026 incident: sheet truncated from 40k+ to ~5k rows; restored from GCS

## Related pages

- [[projects/rattaproff]]
- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
