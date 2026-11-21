---
type: concept
title: "Google Sheets ground-truth sync safety"
product: rattaproff
project: rattaproff
created: 2026-07-13
updated: 2026-11-21
tags: [rattaproff, google-sheets, gcs, robot, data-integrity]
---

# Google Sheets ground-truth sync safety

## Definition

How [[projects/rattaproff]] updates the `Rattaproff Products` Google Sheet (`1tWrtr3hSpwW-jgf-KBVm8oaWMF0ax9-FfC2uJrwXNHU` via `RATTAPROFF_GSHEET_ID`, ~41k SKUs × 65 columns as of the **2026-11-21** slim migration) without truncating it on partial failure, without hitting Drive's storage lock or Google Sheets' 10M-cell limit, and without letting the next robot run trust a mixed old/new worksheet.

## Relevance

The sheet is merchandising ground truth: published flags, pricing, titles/descriptions, and categories (product permalinks moved exclusively to Cloud SQL in the 2026-11-21 slim migration). A truncated or mixed sheet causes wrong WooCommerce change plans on the following run (delisted products can reappear, new products can be missed). Meanwhile, the prior 118-column sheet (`1AGIUKToNIKJqIyQ5mUh-0bP3oKg1_Wlmqfnwmr-Kkiw`, now retired as `Rattaproff Products -- Old (do not use anymore)`) reached `97,375,487` bytes and locked read-only, motivating `RAW` values-only writes and a `78 MiB` (`81,788,928`-byte) cap.

## How it works

**Write (end of robot run only)** — `sync_sheet_with_ground_truth` enforces `SHEET1_COLUMNS` (65 columns), saves the intended dataframe to GCS (`gsheet` prefix, read back with `SHEET1_TEXT_DTYPES`), then `replace_sheet_content_with_df` writes in place in `RAW` values-only mode (no cell formatting or apostrophe escaping):

1. `__sync_status` tab → `in_progress` (tab auto-created on first write)
2. Chunked `RAW` write with `resize=False` (grid never shrinks until all chunks succeed)
3. Trim grid to exact new row/column count
4. `__sync_status` → `complete`
5. Up to 20 full retries on failure; marker stays `in_progress` if all fail

**Rejected approach:** staging/backup tab in the same spreadsheet — would double cell count toward Google's 10M-cell file limit (~2.66M cells in slim `Sheet1` + category tabs vs ~5.3M+ doubled, and ~9.6M+ on the legacy 118-column sheet).

**Read** — `load_current_gsheet` resolves `RATTAPROFF_GSHEET_ID` (failing loudly on zero/ambiguous title matches) and checks `__sync_status`. If `in_progress`, it falls back to the GCS snapshot saved immediately before the failed write. `df_from_storage` uses text dtypes for IDs, EANs, and image IDs to keep leading zeros. Legacy sheets without `__sync_status` load normally with a warning.

**Same-day shops:** Woo sync uses in-memory `gsheet_new_df` before the sheet write; partial sheet state does not affect the current run's storefront updates.

## Projects using it

- [[projects/rattaproff]] — `gsheet.py`, `gsheet_write.py`, `sheet_utils.py`, `process.py`

## Sources

- [gsheet-ground-truth-sync.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-ground-truth-sync.md)
- [gsheet-slim-migration.md](file:///Users/andreskull/rattaproff/docs/architecture/features/gsheet-slim-migration.md)
- June 2026 incident: sheet truncated from 40k+ to ~5k rows; restored from GCS; Nov 2026 storage lock at `97.4 MB` resolved via 65-column slim migration (`81.0 MB` vs `78 MiB` cap)

## Related pages

- [[projects/rattaproff]]
- [[products/rattaproff]]
- [[concepts/huf-eur-pipeline-pricing]]
