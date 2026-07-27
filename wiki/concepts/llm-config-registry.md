---
type: concept
title: "LLM Config Registry"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-07-27
tags: [llm, configuration, registry, experimentation, multi-model]
---

# LLM Config Registry

A general-purpose, code-based configuration registry in [[projects/gor_dagster]] for managing multi-model LLM experimentation across all AI-powered features.

## What it is

Rather than hardcoding model names and prompts into individual assets, all LLM configurations (model provider, parameters, prompt references, supported use cases) live in a central registry. Assets reference a `llm_config_id` string; the registry resolves this to the actual model and prompt at runtime.

## Why it matters

- **Zero data loss during evolution** — existing results remain valid as the registry grows
- **Unlimited experimentation** — any provider/model combination can be added without code changes
- **Perfect traceability** — every result is tied to its exact config ID and parameters
- **Unified cost tracking** — all LLM spend flows through one system
- **Cross-feature reuse** — the same config used for speaker attribution can be benchmarked for facts extraction

## Config IDs in use

**Production pipeline (2026-07-27):** **`si-gem35fl-recursive`** (@450s) / **`fe-gem35fl-recursive`** (@1800s) — Gemini 3.5 Flash-Lite memory-centric recursive SI/FE. See [gemini-35-flash-lite-migration.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/gemini-35-flash-lite-migration.md) and [recursive-llm-extraction.md](file:///Users/andreskull/gor_dagster/docs/architecture/features/recursive-llm-extraction.md).

**Prior production / readable:** **`si-gem31fl-recursive`** / **`fe-gem31fl-recursive`** (kept in source-priority + ActionableSignal priority 2)

**Compat / experimentation tails:** `si-dsv4fr-58k`, `fe-dsv4fr-58k`, grok-era `si-grok-*` / `fe-grok-*`, `fe-gpt-5.2`, `tk_gem35fl_*` / `tk_gem31fl_*` token-ceiling grids

**LinkedIn discovery:** `linkedin-gemini-flash` → model `gemini-3.5-flash-lite` (2026-07-27)

**Legacy speaker attribution eval configs:** `gemini`, `gemini_20`, `gemini_25_pro`, `gpt5`, `gpt4o`, `gpt4o_mini`, `o3_mini`, `claude4_sonnet`

## Ladder surfaces (do not conflate)

| Surface | Role |
|---------|------|
| In-pass model fallback | `RECURSIVE_MODEL_FALLBACK_CHAINS_DEFAULT` — retry a pass with next model |
| SI/FE job retry | `PIPELINE_*_RETRY_CONFIG_IDS` — next job attempt after failure |
| Source-priority | Which existing artefact wins for HY/FE consumption |

## GCS naming convention

Results are stored with config ID in the path:
`gs://[bucket]/[type]/{episode_id}/{stt_provider}_{llm_config_id}.json`

## Projects using it

- [[projects/gor_dagster]]

## Related pages

- [[concepts/speaker-attribution]]
- [[concepts/actionable-signal]]
- [[concepts/linkedin-enrichment]]
- [[entities/dagster]]
