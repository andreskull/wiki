---
type: project
title: "ai-factory"
product: null
project: ai-factory
created: 2026-11-12
updated: 2026-11-12
tags: [methodology, ai-coding, claude-code, cursor, antigravity, spec, executor, multi-model]
---

# ai-factory

Not a product repo. The lean, multi-model AI coding workflow and shared rule/skill system at `/Users/andreskull/ai-factory` that governs development across Claude Code, Cursor, and Antigravity. Replaced [[projects/spec-driven-ai-coding]] and `~/.ai_global` on **2026-11-12** (Phases 0–5 complete).

## Purpose and role

Provides one shared working contract ([`AGENTS.md`](file:///Users/andreskull/ai-factory/AGENTS.md)), ten user-level `/` skills, two read-only cross-model/cross-vendor verification agents (`critic` and `reviewer`), deterministic Python helper scripts, and thin stack lessons (`rules/stacks/*.md`) linked into `~/.claude`, `~/.cursor`, and `~/.gemini/config` via `scripts/install.py`. Each switched product repo carries only its own project facts in a concise root `AGENTS.md` (imported by a one-line `@AGENTS.md` `CLAUDE.md`).

## Tech stack / tooling

- **Harnesses supported:** Claude Code (`claude`), Cursor IDE + CLI (`cursor` / `agent`), Antigravity IDE + CLI (`antigravity` / `agy`)
- **Language & tests:** Python 3 standard-library scripts (`scripts/*.py`) + `unittest` suite (`233` unit tests in `tests/`, plus live CLI canary/scenario probes)
- **Global safety hook:** [`hooks/block_inline_python.sh`](file:///Users/andreskull/ai-factory/hooks/block_inline_python.sh) blocks `python -c`, heredocs fed to Python, and pipes into Python across all three tools

## Installed skills (`/` commands)

| Skill | Purpose |
|---|---|
| `/spec` | Two-tier feature spec: `brief.md` for small changes or `requirements.md` (+ static `prototype/index.html` for UI changes), reviewed by `critic` on another model |
| `/plan` | Writes `design.md` (reviewed by `critic`) and `tasks.md` leaf checklist (checked by `check_feature.py` + light `critic`) |
| `/executor` | Implements ready leaves in order (`next_leaf.py`), runs each leaf's verify command into `logs/`, commits per proven leaf (`stage_own_changes.py`) or writes `.patch` when `Agent branch: none`, runs cross-model `reviewer`, records `runs.md`, and pushes when `push_policy.py` allows |
| `/review` | Runs `reviewer` on a diff against its acceptance criteria on a model/vendor that did not write the change |
| `/audit` | Read-only gap check across feature docs, leaf ticks, and code |
| `/wrapup` | Runs `/audit`, extracts permanent `docs/architecture/features/<name>.md` with Evidence, updates `WIKI.md`, deletes `docs/features/<name>/` after owner confirmation, stages only its own edits (`stage_own_changes.py`), commits, and pushes if allowed (`push_policy.py`) |
| `/cleanup` | Lists scratch logs, debug artifacts, and finished one-time scripts; deletes only after second confirmation |
| `/improve` | Proposes one factory rule, script check, or hook from a repeated failure in `runs.md` / `docs/ledger.md` |
| `/factory` | Prints available factory commands, how to invoke them, and which model each tool resolves right now (`factory_help.py`) |
| `/humanizer` | Rewrites AI-drafted prose into natural human writing for blog posts, LinkedIn, X/Twitter, or Reddit |

## Key scripts and architecture

- **Cross-vendor model routing ([`scripts/resolve_model.py`](file:///Users/andreskull/ai-factory/scripts/resolve_model.py), [`config/models.json`](file:///Users/andreskull/ai-factory/config/models.json), [`scripts/launch_agent.py`](file:///Users/andreskull/ai-factory/scripts/launch_agent.py)):** Chooses a different vendor (or different model family within Claude Code) for `critic` and `reviewer` (e.g., Grok ↔ Opus in Cursor, Gemini 4 Carbon ↔ Opus in Antigravity), tracks quota resets (`--mark-unavailable`), and spawns read-only CLI subprocesses when a harness's native subagent tool cannot target the resolved model.
- **Install & health check ([`scripts/install.py`](file:///Users/andreskull/ai-factory/scripts/install.py), [`scripts/doctor.py`](file:///Users/andreskull/ai-factory/scripts/doctor.py)):** Dry-run by default; backs up replaced files to `~/.ai-factory-backup/<timestamp>/` before linking skills, agents, rules, and hooks. `doctor.py` validates global links/hooks and `doctor.py --repo <path>` audits switched repos for legacy `.cursorrules`, always-apply `.mdc` files, loose `.cursor/*.md`, or stale `~/.ai_global` links.
- **Git & index safety ([`scripts/push_policy.py`](file:///Users/andreskull/ai-factory/scripts/push_policy.py), [`scripts/stage_own_changes.py`](file:///Users/andreskull/ai-factory/scripts/stage_own_changes.py)):** Reads `- Agent branch:` (`development`, `staging`, `main`, or `none`) and `- Remote:` from a switched repo's `AGENTS.md` to decide push eligibility deterministically; stages index-only diffs from pre-edit snapshots so unrelated unstaged work in shared files (`WIKI.md`, `botastico-api` working tree) is never swept into a commit.
- **Stack lessons (`rules/stacks/`):** Concise non-obvious failure lessons only (`bigquery.md`, `dagster.md`, `dash.md`, `nextjs.md`, `python.md`, `shadcn.md`, `typescript.md`, `vercel.md`). Project-specific Antigravity MCP servers (such as Firebase on Botastico repos) live in repo-scoped `.agents/plugins/<name>/` rather than globally.

## Switched repositories (Phase 4 — completed 2026-11-12)

| Repo | Agent branch | Notes |
|---|---|---|
| `finfluencer-tracker` | `development` | 87-line `AGENTS.md`; extracted `docs/ops/{deployment-previews,performance-budget}.md` & `docs/design-system/agent-conventions.md` |
| `gor-blog` | `development` | 48-line `AGENTS.md`; `mkdocs.yml` `exclude_docs: features/` verified by `npm test` and `doctor.py` |
| `gor_dagster` | `staging` | 80-line `AGENTS.md` + 10 extracted architecture/ops docs; safe `pytest` selection & 3-way BigQuery DDL rule |
| `botastico-portal` | `development` | 36-line `AGENTS.md`; `apps/client` (`:3000`) primary app, client/admin lint + `turbo` build checks |
| `botastico-api` | `none` | 36-line `AGENTS.md`; single-branch `main` unstaged-until-production workflow; `static.yml` & `gh-pages` retired |
| `wiki` | `main` | 248-line `AGENTS.md` operating manual; `draft-commands/` retired |

## Current status

- **Phases 0–5 complete (2026-11-12):** All six active repos switched and verified across Claude Code, Cursor, and Antigravity; `/humanizer` added; `~/.ai_global` and `spec-driven-ai-coding` retired (`~/.ai-factory-backup/20261112T162709Z-phase5`).
- **Next (Phase 6):** Accumulate real feature `runs.md` records across switched repos to populate `docs/ledger.md` and run `/improve`.

## Key docs

- [`AGENTS.md`](file:///Users/andreskull/ai-factory/AGENTS.md)
- [`docs/user-guide.md`](file:///Users/andreskull/ai-factory/docs/user-guide.md)
- [`docs/decisions.md`](file:///Users/andreskull/ai-factory/docs/decisions.md)
- [`docs/implementation-plan.md`](file:///Users/andreskull/ai-factory/docs/implementation-plan.md)
- [`docs/project-inventory.md`](file:///Users/andreskull/ai-factory/docs/project-inventory.md)
- [`docs/test-log.md`](file:///Users/andreskull/ai-factory/docs/test-log.md)

## Related pages

- [[concepts/spec-driven-development]]
- [[projects/spec-driven-ai-coding]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[projects/botastico-portal]]
