---
type: source
title: "Cramer / Mad Money — performance & methodology (internal research)"
product: finfluencer-trade
project: gor-blog
created: 2026-04-07
updated: 2026-04-07
tags: [cramer, mad-money, finfluencer, methodology, academic, inverse-cramer]
---

# Cramer / Mad Money — performance & methodology (internal research)

## Source

- **Working copy (repo):** [Cramer's Performance_ Research & Methodology.md](file:///Users/andreskull/gor-blog/research/cramer/Cramer's%20Performance_%20Research%20%26%20Methodology.md) under **`gor-blog/research/cramer/`**
- **Vault archive (immutable):** [cramer-mad-money-performance-methodology.md](file:///Users/andreskull/wiki/raw/papers/cramer-mad-money-performance-methodology.md) in `wiki/raw/papers/` (synced from the repo path above)
- **Ingested:** 2026-04-07; path updated 2026-04-07 when the file moved from `_internal/papers/` → **`research/cramer/`**

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
- **Publication:** [[projects/gor-blog]] — repo research under **`research/`** feeds eventual `docs/blog/posts/`; this page is the vault-side record.

## Contradictions / open points

- Document contains `[User Query]` placeholders and draft markers — treat as **research draft**, not peer-reviewed publication.
- Images in the original are external references (`![][image1]` etc.) — figures may not render in vault copy without assets.

## Related pages

- [[projects/gor-blog]]
- [[projects/gor_dagster]]
- [[concepts/actionable-signal]]
- [[synthesis/lint-gor-blog-internal-papers-2026-04-07]]
