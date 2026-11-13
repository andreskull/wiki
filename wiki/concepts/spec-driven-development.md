---
type: concept
title: "Spec-Driven Development"
product: null
project: ai-factory
created: 2026-04-06
updated: 2026-11-13
tags: [methodology, spec, planning, executor, ai-coding, workflow, multi-model]
---

# Spec-Driven Development

The AI-assisted development methodology used across all of Andres's repositories across Claude Code, Cursor, and Antigravity. Powered since **2026-11-12** by [[projects/ai-factory]] (replacing [[projects/spec-driven-ai-coding]] and `~/.ai_global`).

## The loop

```
/spec     → brief.md (small) OR requirements.md (+ static prototype/index.html for UI) + cross-model critic
/plan     → design.md (+ critic) → tasks.md leaf checklist (+ check_feature.py & light critic)
/executor → ready leaves in order (next_leaf.py), verify cmd → logs/, index-only commit per leaf (stage_own_changes.py), cross-model reviewer, runs.md, push if push_policy.py allows
/wrapup   → /audit + permanent docs/architecture/features/<name>.md + WIKI.md + confirm delete docs/features/<name>/ + commit/push
wiki sync → update ~/wiki project & concept pages
```

All ten commands also accept a slashless `<skill>:` prefix (`spec:`, `humanizer:`, `plan:`, etc.) and two-step empty `/spec` or `/humanizer` invocations for chat composers (such as Claude Desktop's Code tab) that block leading `/` commands containing links or code blocks.

## Key principles

- **Two tiers, lean artifacts:** Small changes use a single `brief.md` (intent, scope, criteria, leaves); larger features use `requirements.md` (`AC-N WHEN ... THE SYSTEM SHALL ... [verify: unit|integration|e2e|manual]`), `design.md`, and `tasks.md`. UI features include a throwaway static `prototype/index.html` at the intent gate.
- **Cross-vendor / clean-context verification:** `resolve_model.py` and `launch_agent.py` run `critic` (documents) and `reviewer` (code) on a different vendor or model family than the author in a clean non-forked context (never inheriting the author's conversation; `"author_only": true` models like `claude-fable-5-2` are recognized as authors without ever receiving review or leaf tasks).
- **Reversible autonomy inside leaves:** Once requirements and `tasks.md` are approved, `/executor` makes reversible implementation choices within a leaf's acceptance criteria on its own and notes them in `runs.md`, stopping only on scope/contract changes, 3 verify failures, or destructive/production actions.
- **Repo state is portable across tools:** `docs/features/<name>/` (`runs.md`, `logs/`, leaf ticks) holds all state so work can move seamlessly between Claude Code, Cursor, and Antigravity if one tool hits usage limits.
- **One clean instruction file per repo:** Each switched repo has a concise root `AGENTS.md` (`CLAUDE.md` = `@AGENTS.md`) checked by `doctor.py --repo`; `ai-factory` itself keeps its repo facts in `REPO.md` (`CLAUDE.md` = `@REPO.md`) so product repos never load `ai-factory`'s own commands or branch rule.
- **Safe staging and push rules:** `stage_own_changes.py` stages only the agent's own diff hunks (never sweeping unrelated unstaged edits), and `push_policy.py` enforces each repo's `Agent branch` (`development`, `staging`, `main`, or `none`).

## Where it lives

[[projects/ai-factory]] (`/Users/andreskull/ai-factory`) — `AGENTS.md`, `REPO.md`, `skills/`, `agents/`, `scripts/`, `rules/stacks/`, `hooks/block_inline_python.sh`.

## Projects switched (7 repos as of 2026-11-13)

`gor_dagster` (`staging`), `finfluencer-tracker` (`development`), `gor-blog` (`development`), `botastico-portal` (`development`), `botastico-api` (`none` — unstaged on `main` until production), `wiki` (`main`), and `ai-factory` (`main`, via `REPO.md`).

## Related pages

- [[projects/ai-factory]]
- [[projects/spec-driven-ai-coding]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[projects/botastico-portal]]
