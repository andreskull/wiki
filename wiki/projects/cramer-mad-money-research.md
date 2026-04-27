---
type: project
title: "cramer-mad-money-research"
product: finfluencer-trade
project: cramer-mad-money-research
created: 2026-04-16
updated: 2026-04-27
tags: [cramer, mad-money, research, reproducibility, csv, finfluencer, public-data, ssrn, working-paper]
---

# cramer-mad-money-research

Public reproducibility repository for the **Jim Cramer / *Mad Money*** study (2018–2024): **16,701** long recommendations, frozen CSV/Parquet, analysis scripts, figures, and working paper. **Published on SSRN** as:

> *What Cramer Holds vs What He Recommends: Signal-Time Features in 16,701 Mad Money Recommendations (2018–2024)* — [SSRN 6643379](https://ssrn.com/abstract=6643379)

**GitHub:** [github.com/andreskull/cramer-mad-money-research](https://github.com/andreskull/cramer-mad-money-research)

Sibling to [[projects/gor-blog]] and [[projects/gor_dagster]]; **not** part of the MkDocs site — a standalone public repo.

## Product

[[products/finfluencer-trade]]

## Purpose and role

Ships the **published dataset and analysis code** so third parties can verify methodology (T+1 open, dual SPY/QQQ benchmarks, implicit close, sequence model, hold subtype classification). Snapshot CSVs are produced from [[projects/gor_dagster]] (BigQuery view `CramerResearchSnapshot_2018_2024`) using private export scripts in **`gor-blog/research/cramer/scripts/`** — those scripts are **not** in this repo. Internal research notes (`research_plan.md`, `SPEC_sequence_model.md`) also live only under `gor-blog/research/cramer/`.

## Tech stack

- **Language:** Python (pandas, scipy, statsmodels, matplotlib; optional `pyarrow` for Parquet)
- **Data:** CSV, Parquet; no database access required to reproduce analysis from the bundled snapshots
- **Hosting:** Public GitHub; PDF built with `pandoc` + `xelatex` via `scripts/build_pdf.py` (optional)

## Current status (2026-04-25)

- **SSRN** abstract approved; PDF revised on SSRN to match the repo build (SSRN line on title page, Data Availability).
- **ORCID** — author added the work; optional Scholar profile claim remains.
- **Private** pipeline exports and spec drafts: [[projects/gor-blog]] `research/cramer/`.

## Repo path

[cramer-mad-money-research](file:///Users/andreskull/cramer-mad-money-research) — root `README.md` describes layout and run order.

## Relationship to other repos

| Repo | Role |
|------|------|
| [[projects/gor_dagster]] | BigQuery view `CramerResearchSnapshot_2018_2024`, deploy script `scripts/deploy_cramer_research_snapshot.py` |
| [[projects/gor-blog]] | MkDocs site; **private** `research/cramer/` — BigQuery export scripts, `SPEC_*.md`, `SSRN_submission.md` (no archived extras folder; see git history) |
| This repo | Static CSV/Parquet + **analysis-only** scripts + paper (Markdown + PDF) — no warehouse credentials |

## Wiki integration

The public repo is indexed here from its root `README.md` and `paper/` (per vault rules). A root `WIKI.md` is optional; the vault does **not** require mirroring this repo’s tree into `wiki/`. The vault’s narrative entry is this page and [[sources/2026-04-07-cramer-mad-money-performance-methodology]].

## Related pages

- [[sources/2026-04-07-cramer-mad-money-performance-methodology]] — methodology source notes
- [[projects/gor-blog]] — site and publication layer
- [[projects/gor_dagster]] — pipeline and BigQuery views
- [[concepts/signal-performance]] — horizons and implicit close
