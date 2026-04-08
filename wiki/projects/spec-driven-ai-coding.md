---
type: project
title: "spec-driven-ai-coding"
product: null
project: spec-driven-ai-coding
created: 2026-04-06
updated: 2026-04-06
tags: [methodology, ai-coding, cursor, antigravity, spec, planning, workflow]
---

# spec-driven-ai-coding

Not a product repo. The methodology and tooling that governs how all products are built — a structured, spec-first AI development workflow used across all projects with Cursor and Antigravity.

## Purpose and role

Defines the development process: requirements → design → tasks → implementation → wrapup. Provides global AI commands (`.ai_global/commands/`) and rules (`.ai_global/rules/`) that are synced to `~/.ai_global/` and available as `/` commands in all projects.

## Tech stack / tooling

- **AI tools:** Cursor, Antigravity, Claude (Cowork)
- **Command format:** Markdown prompt files in `.ai_global/commands/` and `prompts/`
- **Sync mechanism:** `./sync-ai-global.sh` pushes `.ai_global/` → `~/.ai_global/`
- **Project-level rules:** Each project's `.ai-rules/` and `.cursor/` reference the global commands

## Current commands

| Command | File | Purpose |
|---|---|---|
| `/spec` | `spec.md` | Creates `docs/features/[name]/requirements.md` + README.md |
| `/plan` | `plan.md` | Phase 1 → `design.md`; Phase 2 → `tasks.md` |
| `/executor` | `prompts/executor.md` | Implements one task at a time |
| `/audit` | `audit.md` | System health: spec/design/code alignment |
| `/cleanup` | `cleanup.md` | Dry-run + confirm deletion of dev artifacts |
| `/promote` | `promote.md` | Promotes project rule to global `~/.ai_global/rules/` |
| `/steering` | `prompts/steering.md` | Creates/updates `.ai-rules/product.md`, `tech.md`, `structure.md` |
| `/wrapup` | `wrapup.md` | Feature completion: extract docs, update WIKI.md, clean temp files |

Legacy commands in `.ai_global/commands/_legacy/`: `planner.md`, `executor.md`, `steering.md`

## Feature lifecycle

```
/spec → requirements.md + README.md
/plan → design.md (Phase 1)
/plan → tasks.md (Phase 2)
/executor → implement (one task per run)
/wrapup → extract to docs/architecture/features/, update WIKI.md, delete feature folder
wiki sync → update wiki project page
```

## Global rules

`.ai_global/rules/` contains framework-specific standards: `bigquery-sql-standards.md`, `dagster-standards.md`, `python-standards.md`, `nextjs-standards.md`, `typescript-standards.md`, `javascript-standards.md`, `shadcn-standards.md`, `dash-standards.md`, plus workflow enforcement rules (`planner-gate.md`, `executor-gate.md`, `spec-workflow-enforcement.md`, `task-manager.md`, `requirements-format.md`, `project-context.md`).

## Docs

- [AI Workflow](file:///Users/andreskull/spec-driven-ai-coding/docs/AI_WORKFLOW.md)
- [WIKI.md](file:///Users/andreskull/spec-driven-ai-coding/WIKI.md)

## Related pages

- [[concepts/spec-driven-development]]
- [[entities/cursor]]
- [[entities/antigravity]]
