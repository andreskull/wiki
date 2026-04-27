---
type: source
title: "Cramer / Mad Money — performance & methodology (internal research)"
product: finfluencer-trade
project: gor-blog
created: 2026-04-07
updated: 2026-04-27
tags: [cramer, mad-money, finfluencer, methodology, academic, inverse-cramer, ssrn]
---

# Cramer / Mad Money — performance & methodology (internal research)

## Source

- **Public reproducibility repo + working paper:** [[projects/cramer-mad-money-research]] — [cramer-mad-money-research](file:///Users/andreskull/cramer-mad-money-research) (CSV/Parquet, analysis-only scripts, figures, paper). **SSRN** [6643379](https://ssrn.com/abstract=6643379).
- **Private prep (not public):** [gor-blog/research/cramer/](file:///Users/andreskull/gor-blog/research/cramer/) — `research_plan.md`, `SPEC_*.md`, `SSRN_submission.md`, and scripts that export from BigQuery / run full hold reclassification (`enrich` / `extract`). Superseded QQQ-era scripts were removed from the tree in 2026-04; **git history** retains them.
- **Vault archive (immutable):** [cramer-mad-money-performance-methodology.md](file:///Users/andreskull/wiki/raw/papers/cramer-mad-money-performance-methodology.md) in `wiki/raw/papers/`
- **Ingested:** 2026-04-07; path history: `_internal/papers/` → **`gor-blog/research/cramer/`** (working area; methodology vault copy is `wiki/raw/papers/…`)

Long-form synthesis tying academic event studies (e.g. Engelberg et al.), AAP / charitable trust benchmarks, inverse ETFs (SJIM/LJIM), and finfluencers.trade-style signal definitions (horizons, next-day open entry, LLM extraction) for Jim Cramer / *Mad Money*.

## Key takeaways

- **Signal taxonomy** — Directional picks map to badges (long/hold/close long; short/hold/close short); high-conviction, imperative picks only; vague commentary excluded — aligns with how a pipeline should define **[[concepts/actionable-signal]]**-style commitments.
- **Horizons** — 1w / 1m / 3m / 6m / 1y trading-day windows; **entry at next session open** after air date avoids phantom alpha from overnight “attention spike” — matches honest retail execution assumptions.
- **Academic “Cramer effect”** — Short-term abnormal returns and reversals; stronger for small caps; **retail attention** and limits to arbitrage explain persistence of mispricing.
- **AAP / long-run** — Hartley & Olson–style stats: underperformance vs S&P on return and Sharpe; factor tilts (small, growth, low-quality earnings), **cash drag** in a charitable trust structure.
- **Inverse products** — SJIM/LJIM launch and **liquidation** (2023–2024): illustrates risk of naive “inverse influencer” products when regime and signal curation differ.
- **Methodology paradox** — “Cramer picks not so bad” vs meme/academia: resolved via **signal curation** (manual vs LLM), **entry price**, and cap/sector weighting; LLM consistency vs human monitoring.
- **LLM risks** — Survey of biases (look-ahead, survivorship, narrative, objective, representation); mitigation via provenance to transcript/audio — relevant to [[projects/gor_dagster]] facts extraction and evaluation.

## Connections

- **Product:** [[products/finfluencer-trade]] — accountability methodology for televised picks.
- **Pipeline:** [[projects/gor_dagster]] — STT → facts extraction → instruments → performance horizons.
- **Publication:** [[projects/gor-blog]] — articles in `docs/blog/posts/`; public Cramer kit is [[projects/cramer-mad-money-research]]; DB-export and internal specs stay under `gor-blog/research/cramer/`. This page is the vault-side methodology record.

## Contradictions / open points

- The **vault** copy in `raw/papers` may still read like an early research memo; the **cited** working paper is the repo Markdown/PDF and **SSRN** PDF (April 2026).
- Some placeholders or draft markers may remain in the archived raw file — the public paper is the authority for claims and counts.
- Images in the original may be external references — figures in the public repo are canonical for the paper.

## Related pages

- [[projects/cramer-mad-money-research]]
- [[projects/gor-blog]]
- [[projects/gor_dagster]]
- [[concepts/actionable-signal]]
- [[synthesis/lint-gor-blog-internal-papers-2026-04-07]]
