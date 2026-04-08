---
type: concept
title: "LLM Config Registry"
product: finfluencer-trade
project: gor_dagster
created: 2026-04-06
updated: 2026-04-06
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

Speaker attribution: `gemini`, `gemini_20`, `gemini_25_pro`, `gpt5`, `gpt4o`, `gpt4o_mini`, `o3_mini`, `claude4_sonnet`
Facts extraction: `fe-gpt-5.2`, `fe-gpt-5`, `fe-grok-4-fast-reasoning-*`

## GCS naming convention

Results are stored with config ID in the path:
`gs://[bucket]/[type]/{episode_id}/{stt_provider}_{llm_config_id}.json`

## Projects using it

- [[projects/gor_dagster]]

## Related pages

- [[concepts/speaker-attribution]]
- [[concepts/actionable-signal]]
- [[entities/dagster]]
