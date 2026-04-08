---
type: concept
title: "Spec-Driven Development"
product: null
project: spec-driven-ai-coding
created: 2026-04-06
updated: 2026-04-06
tags: [methodology, spec, planning, executor, ai-coding, workflow]
---

# Spec-Driven Development

The AI-assisted development methodology used across all of Andres's projects. Structures LLM coding sessions into explicit phases — requirements, design, tasks, implementation, wrapup — to produce documented, maintainable code rather than throwaway scripts.

## The loop

```
/spec  → requirements.md + README.md   (WHAT to build)
/plan  → design.md                      (HOW to build it — Phase 1)
/plan  → tasks.md                       (exact steps — Phase 2)
/executor → one task at a time          (implement)
/wrapup → permanent docs + WIKI.md update + clean temp files
wiki sync → update wiki
```

## Key principles

- **One phase per run** — spec, plan (design), plan (tasks), executor each run separately; no skipping
- **Explicit approval between phases** — each phase requires user review before the next starts
- **Temporary feature files** — `docs/features/[name]/` holds requirements, design, tasks during development. Deleted by `/wrapup` when done.
- **Permanent docs** — `/wrapup` extracts to `docs/architecture/features/[name].md`
- **Real services only** — executor tests use real APIs, real BigQuery, real GCS. No mocks.

## Where it lives

[[projects/spec-driven-ai-coding]] — `.ai_global/commands/` for global commands, `.ai_global/rules/` for framework standards

## Projects using it

All: `gor_dagster`, `rattaproff`, and others

## Related pages

- [[projects/spec-driven-ai-coding]]
- [[entities/cursor]]
- [[entities/antigravity]]
