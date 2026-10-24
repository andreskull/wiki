---
type: decision
title: "Option 3 Hybrid Routing: Claude Haiku 5.5 SI and Gemini 3.5 Flash-Lite FE"
date: 2026-10-24
status: accepted
tags: [architecture, routing, llm, speaker-attribution, facts-extraction, claude, gemini, cost-optimization]
---

# Option 3 Hybrid Routing: Claude Haiku 5.5 SI and Gemini 3.5 Flash-Lite FE

**Date:** 2026-10-24  
**Status:** Accepted and Shipped (`main` PR #230)  
**Context:** Comprehensive empirical evaluation of Anthropic's Claude Haiku 5.5 against Google's Gemini 3.5 Flash-Lite across Speaker Identification (SI) and Facts Extraction (FE).

---

## Decision

Adopt **Option 3 — Hybrid Routing**:
1. **Promote `si-haiku55-recursive`** (@ 450s, 3 waves) as production primary for **Speaker Identification (SI)** at **Rank #1**, replacing `si-gem35fl-recursive`.
2. **Retain `fe-gem35fl-recursive`** (@ 1800s) as production primary for **Facts Extraction (FE)** at **Rank #1**.
3. **Bridge Mapping**: In `gor_dagster/configs/pipeline_eligibility.py`, map `"si-haiku55-recursive": "fe-gem35fl-recursive"` in `PIPELINE_FE_CONFIG_BY_SI_CONFIG_ID`. Episodes hydrated from Claude Haiku 5.5 SI route directly to Gemini 3.5 Flash-Lite for financial prediction extraction.

---

## Rationale

Four routing choices were analyzed against empirical benchmark criteria:

| Metric / Gate | `si-haiku55-recursive` (SI) vs `gem35fl` | `fe-haiku55-recursive` (FE) vs `gem35fl` |
| :--- | :--- | :--- |
| **Quality Score / F1** | **+0.0078** overall (0.7868 vs 0.7790) | **-0.1947** F1 (0.3682 vs 0.5629) |
| **Accuracy / Precision** | **+1.35 pp** speaker accuracy (92.75% vs 91.40%) | **+0.2552** precision (1.0000 vs 0.7448) |
| **Recall / Coverage** | 100% utterance recovery; 0 schema errors | **-0.2000** recall (0.2541 vs 0.4541); 14 vs 35 predictions |
| **Validation Rejections** | 0% parse failures; 0% truncations | **17.65%** rejection rate (3 vs 0 rejections) |
| **Cost Ratio** | **0.6318x** golden eval (-36.8%), **0.475x** calibration (-52.5%) | **0.4696x** per episode (-53.0%) |
| **Wallclock Ratio** | **0.6147x** (-38.5% latency) | **0.7932x** (-20.7% latency) |
| **Gate Status** | **PASS** (met AC1–AC6 gates) | **FAIL** (failed Relative FE Gate on recall & F1) |

- **Why not Option 1 (Full Cutover to Haiku 5.5 SI+FE)?**
  While `fe-haiku55-recursive` achieved perfect precision (1.0000), it proved severely over-conservative on recall (0.2541 vs 0.4541), cutting platform signal generation by 44% and reducing mean F1 to 0.3682. This failed the Relative FE Gate.
- **Why not Option 4 (Hold on Gemini 3.5 Flash-Lite for both)?**
  `si-haiku55-recursive` decisively won the SI benchmark: higher speaker accuracy (+1.35 pp), higher overall alignment score, 40–55% lower cost, and 38–55% faster wallclock runtime with 0.0% schema and truncation errors across hundreds of passes. Holding would leave substantial cost and latency gains unrealized.
- **Option 3 captures the best of both models**: 40–50% cost and speed savings on the high-volume SI stage, while preserving Gemini 3.5 Flash-Lite's high-recall signal extraction on the FE stage.

---

## Consequences & Safety Guardrails

1. **4-Tier SI Fallback Chain**:
   `si-haiku55-recursive` $\to$ `si-gem35fl-recursive` $\to$ `si-gem31fl-recursive` $\to$ `si-dsv4fr-recursive`. If Anthropic suffers an outage or semantic retry failure, the pipeline seamlessly cascades to Google Gemini and DeepSeek.
2. **Memory Fold Question-Answering Boundary**:
   In `gor_dagster/utils/recursive_memory.py`, window passes are prohibited from answering questions for STT speaker subjects not present within their own primary index window, preventing spurious host re-attributions in multi-speaker banter.
3. **Artifact Compatibility**:
   Prior hydrated and extracted artifacts from Gemini and DeepSeek remain fully valid and readable in BigQuery and GCS; `PIPELINE_SI_SOURCE_PRIORITY_CONFIG_IDS` preserves historical readability.
