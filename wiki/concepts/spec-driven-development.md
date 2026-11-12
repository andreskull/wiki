---
type: concept
title: "Spec-Driven Development"
product: null
project: ai-factory
created: 2026-04-06
updated: 2026-11-12
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

## Key principles

- **Two tiers, lean artifacts:** Small changes use a single `brief.md` (intent, scope, criteria, leaves); larger features use `requirements.md` (`AC-N WHEN ... THE SYSTEM SHALL ... [verify: unit|integration|e2e|manual]`), `design.md`, and `tasks.md`. UI features include a throwaway static `prototype/index.html` at the intent gate.
- **Cross-vendor / cross-model verification:** `resolve_model.py` and `launch_agent.py` run `critic` (documents) and `reviewer` (code) on a different vendor or model family than the author (e.g., Grok ↔ Opus in Cursor, Gemini 4 Carbon ↔ Opus in Antigravity).
- **Repo state is portable across tools:** `docs/features/<name>/` (`runs.md`, `logs/`, leaf ticks) holds all state so work can move seamlessly between Claude Code, Cursor, and Antigravity if one tool hits usage limits.
- **One clean instruction file per repo:** Each switched repo has a concise root `AGENTS.md` (with line 1 pointing at `~/ai-factory/AGENTS.md` and `CLAUDE.md` holding `@AGENTS.md`) checked by `doctor.py --repo`.
- **Safe staging and push rules:** `stage_own_changes.py` stages only the agent's own diff hunks (never sweeping unrelated unstaged edits), and `push_policy.py` enforces each repo's `Agent branch` (`development`, `staging`, `main`, or `none`).

## Where it lives

[[projects/ai-factory]] (`/Users/andreskull/ai-factory`) — `AGENTS.md`, `skills/`, `agents/`, `scripts/`, `rules/stacks/`, `hooks/block_inline_python.sh`.

## Projects switched (all 6 active repos as of 2026-11-12)

`gor_dagster` (`staging`), `finfluencer-tracker` (`development`), `gor-blog` (`development`), `botastico-portal` (`development`), `botastico-api` (`none` — unstaged on `main` until production), and `wiki` (`main`).

## Related pages

- [[projects/ai-factory]]
- [[projects/spec-driven-ai-coding]]
- [[projects/gor_dagster]]
- [[projects/finfluencer-tracker]]
- [[projects/botastico-portal]]
