# LLM Wiki — Project Requirements

**Author:** Andres Kull
**Date:** 2026-04-06
**Status:** Draft v4 — Final
**Wiki location:** `/Users/andreskull/wiki`
**Spec-driven location:** `/Users/andreskull/spec-driven-ai-coding`

---

## 1. Overview

This document specifies the requirements for Andres's personal LLM-maintained wiki, and the changes needed to the `spec-driven-ai-coding` methodology to connect the two systems. The wiki is a persistent, compounding knowledge base that unifies external reading material and internal software project repos. A single wiki sits on top of all of it. Andres reads and navigates it; the LLM writes and maintains it.

The wiki is accessed from three tools: **Claude** (Cowork/desktop) for knowledge operations, **Cursor** for code development, and **Antigravity** for structured AI-assisted development. All three read the same shared schema. The connection between the dev workflow and the wiki runs through a new `/wrapup` command that runs after each feature is complete.

---

## 2. Products and Projects

### 2.1 Product hierarchy

There is a product layer above individual git repos. A product is a user-facing thing being built. A project is a single git repository that is a component of a product.

| Product | Domain | Repos |
|---|---|---|
| **finfluencer.trade** | Financial influencer tracking | `gor_dagster`, `gor-blog`, `finfluencer-tracker` |
| **botastico** | (to describe during bootstrap) | `botastico`, `botastico-api`, `botastico-script`, `botastico-portal`, `botastico-stripe`, `botastico-script-inserter`, `botastico-opiq-case-study` |
| **rattaproff** | (to describe during bootstrap) | `rattaproff` |

All repos are at `/Users/andreskull/[repo-name]`.

`spec-driven-ai-coding` at `/Users/andreskull/spec-driven-ai-coding` is not a product repo — it is the methodology project, tracked in the wiki as its own entity.

### 2.2 Shared code
No shared libraries between products.

---

## 3. Wiki Directory Structure

```
/Users/andreskull/wiki/          ← Obsidian vault root + git repo
├── CLAUDE.md                    # Schema: operating manual for all LLM tools
├── index.md                     # Master content catalog (updated on every operation)
├── log.md                       # Append-only chronological operation log
│
├── raw/                         # External sources — immutable after ingestion
│   ├── articles/                # Web articles (Obsidian Web Clipper markdown)
│   ├── papers/
│   ├── notes/
│   ├── data/
│   └── assets/                  # Images (deferred)
│
├── wiki/                        # LLM-generated pages — LLM owns this entirely
│   ├── overview.md              # High-level synthesis; first page read per session
│   ├── products/
│   │   ├── finfluencer-trade.md
│   │   ├── botastico.md
│   │   └── rattaproff.md
│   ├── projects/
│   │   ├── gor_dagster.md
│   │   ├── gor-blog.md
│   │   ├── finfluencer-tracker.md
│   │   ├── botastico.md
│   │   ├── botastico-api.md
│   │   ├── botastico-script.md
│   │   ├── botastico-portal.md
│   │   ├── botastico-stripe.md
│   │   ├── botastico-script-inserter.md
│   │   ├── botastico-opiq-case-study.md
│   │   ├── rattaproff.md
│   │   └── spec-driven-ai-coding.md
│   ├── concepts/
│   ├── entities/
│   ├── decisions/
│   └── synthesis/
│
└── outputs/
    ├── reports/
    └── slides/
```

---

## 4. Internal Project Folders — Current State and What Gets Added

### 4.0 As-found docs structure per repo

Scanned 2026-04-06. Botastico repos not yet accessible — to be confirmed during bootstrap.

| Repo | docs/ state | .ai-rules/ | CLAUDE.md | Notes |
|---|---|---|---|---|
| `gor_dagster` | **Rich** — `architecture/` (30+ files), `features/` (6 active), `operations/` (40+ files), `schemas/` (10+ files), `analytics/`, `design/`, `product/` | ✅ populated | ✅ exists | Most mature repo; already fully spec-driven |
| `gor-blog` | **Special** — `docs/` IS the MkDocs site source: `blog/posts/` (14 published articles), `assets/`, `directory/` | ❌ | ❌ | Published blog posts live at `docs/blog/posts/`. No project docs separate from site content. |
| `finfluencer-tracker` | **Minimal** — one file: `docs/auth-sharing-landing-app.md` | ❌ | ❌ | Needs full docs scaffold |
| `rattaproff` | **Scaffold only** — `docs/architecture/` (empty), `docs/features/database-sync/requirements.md` (one active feature), `docs/operations/` (empty), `docs/schemas/` (empty); plus flat legacy docs in `docs/` root | ✅ populated | ❌ | Structure exists, no content yet |
| `spec-driven-ai-coding` | **Minimal** — `docs/AI_WORKFLOW.md`, `docs/WORKFLOW_CLARIFICATION.md` | ❌ | ❌ | Methodology project, not a product repo |
| `botastico*` | Unknown — not yet accessible | Unknown | Unknown | Check during bootstrap |

**Active feature folders not to be indexed** (temporary spec-driven work in progress in `gor_dagster`):
`batch-integration`, `finfluencer-affiliations-human-curation`, `instrument-resolution-bulk-manual-curation`, `pipeline-model-priority-retries`, `post-mvp-loops-migration`, `proof-segment-speaker-resolution`, `social-share-previews`

**Observations for wiki bootstrap:**
- `gor_dagster` can be indexed immediately from its rich existing docs — this will produce the most content by far
- `gor-blog` is the natural home for published articles and blog content. Its `docs/blog/posts/` are work products (published writing), not project docs. The wiki will index these as content outputs, not architecture docs.
- `gor_dagster/docs/analytics/` and `docs/design/` show the pattern already emerging: research/analysis and marketing/design content lives organically in the project repo — no separate wiki `work/` folder needed. This validates the decision in §1.
- `rattaproff` and `finfluencer-tracker` need `WIKI.md` and docs scaffolding created during bootstrap.

### Projects stay in their own locations. The wiki makes two additions to each project:

### 4.1 `WIKI.md` — bridge file (root of each repo)

The LLM creates this; the `/wrapup` command maintains it. It is the entry point for wiki ingestion of that project.

```markdown
# [Project Name] — Wiki Entry Point

## Product
[Which product this repo belongs to]

## What this project is
[1-3 sentence description]

## Key paths
- `src/` — [description]
- `docs/architecture/` — permanent architecture docs
- `docs/features/` — active feature docs (temporary, cleaned by /wrapup)
- `docs/operations/` — deployment and ops docs
- `docs/schemas/` — data schema docs

## Technology stack
[Key frameworks, services, external dependencies]

## Key concepts
[Bullet list of domain concepts]

## Current status
Last updated: YYYY-MM-DD
[in development / stable / deprecated]

## Architecture decisions
<!-- Append-only: YYYY-MM-DD | decision | rationale -->

## Completed features
<!-- Append-only: YYYY-MM-DD | feature name | docs/architecture/features/feature-name.md -->

## Wiki page
[[projects/[project-name]]]
```

### 4.2 `docs/` folder structure

The baseline spec-driven structure — already fully present in `gor_dagster`, partially in `rattaproff`, to be created in others:

```
docs/
├── architecture/       # Permanent: component design, data models, ADRs, completed features
├── features/           # Temporary: active feature work (requirements, design, tasks)
├── operations/         # Permanent: deployment, config, runbooks
└── schemas/            # Permanent: data schema definitions
```

`gor_dagster` additionally has `docs/analytics/` (evaluation results, analysis), `docs/design/` (brand assets, copy matrix), and `docs/product/` (app contract). These organic additions are valid — projects can grow additional subdirectories as needed. The wiki indexes whatever is in `docs/` **except** `docs/features/` (temporary).

`gor-blog` is the exception: its `docs/` is the MkDocs site itself. Published blog posts at `docs/blog/posts/` are the work product — the wiki indexes these as content outputs alongside the project page.

The wiki indexes: `WIKI.md` + all of `docs/` except `docs/features/`. It never indexes `docs/features/`.

---

## 5. CLAUDE.md — Shared Schema

`/Users/andreskull/wiki/CLAUDE.md` is the operating manual for all three LLM tools. Every session begins by reading it. It specifies:

- Product/project map with paths
- Wiki directory layout
- Page type conventions and required sections
- Frontmatter schema
- All operation workflows (ingest, sync, query, lint, wrapup)
- Log entry format and grep patterns
- Cross-reference conventions
- Bootstrap procedure for first run

**Project-level CLAUDE.md files** exist in each repo (used by Cursor and Antigravity). They reference the wiki's `CLAUDE.md` for wiki conventions and add project-specific context. The wiki's `CLAUDE.md` is the authoritative source; project-level files extend it.

---

## 6. Wiki Page Types

### Products (`/wiki/wiki/products/`)
What the product does, target user, component repos with one-line descriptions, overall architecture, current status, key decisions, links to all project pages.

### Projects (`/wiki/wiki/projects/`)
Which product it belongs to, purpose and role, tech stack, architecture summary, key decisions, completed features (with links to `docs/architecture/features/`), current status, link to `WIKI.md`.

### Sources (`/wiki/wiki/sources/`)
One page per ingested external source: title, URL, date, key takeaways, connections to existing projects/concepts, contradictions or updates.

### Concepts (`/wiki/wiki/concepts/`)
Domain concept pages: definition, relevance to Andres's work, which projects use it, which sources discuss it, related concepts.

### Entities (`/wiki/wiki/entities/`)
Named tools, libraries, services (Dagster, Cursor, Antigravity, Stripe, BigQuery, etc.): what it is, how it's used, which projects depend on it.

### Decisions (`/wiki/wiki/decisions/`)
Cross-product architecture decisions: context, options considered, decision made, date, consequences, affected projects.

### Synthesis (`/wiki/wiki/synthesis/`)
Analyses, comparisons, explorations. The LLM decides independently whether to file an answer as a synthesis page, and chooses the title, structure, and location without being asked. No instruction from Andres needed.

### Overview (`/wiki/wiki/overview.md`)
Narrative summary of everything: products, focus areas, recent learnings. Updated during lint passes. First page read by any new session.

---

## 7. Special Files

### `index.md`
Catalog of all wiki pages by type. Each entry: title, wikilink, one-line summary, date updated. LLM reads this at the start of every session to navigate. Updated on every write operation.

Entry format: `- [[projects/gor_dagster]] — Dagster pipeline for finfluencer.trade. Updated 2026-04-01.`

### `log.md`
Append-only record of all wiki operations. Parseable prefix on every entry:

```
## [YYYY-MM-DD] ingest | Source title
## [YYYY-MM-DD] sync | product-or-project-name
## [YYYY-MM-DD] wrapup | project-name | Feature name
## [YYYY-MM-DD] lint | full
## [YYYY-MM-DD] lint | botastico
## [YYYY-MM-DD] query-filed | Synthesis page title
```

---

## 8. Operations

### 8.1 Ingest (external source)
Triggered: Andres drops a file in `raw/` and mentions it (no special syntax needed).

1. LLM reads `index.md`
2. LLM reads the source; discusses key takeaways with Andres
3. LLM creates `wiki/sources/[slug].md`
4. LLM updates all relevant product, project, concept, entity pages; flags contradictions
5. LLM updates `index.md`
6. LLM appends to `log.md`

### 8.2 Sync (internal project or product)
Triggered manually, scoped to a specific project or product.

1. LLM reads the project's `WIKI.md` and relevant `docs/architecture/` files
2. LLM updates the wiki project/product page(s)
3. LLM updates affected concept, entity, decision pages
4. LLM updates `index.md`
5. LLM appends to `log.md`

### 8.3 Queries — ambient wiki awareness
No command needed. When Andres asks anything touching project or domain knowledge, the LLM reads the index, finds relevant pages, and answers with citations. The LLM files substantial answers as synthesis pages on its own judgment.

### 8.4 Lint
Two scopes:

- `lint wiki` — full health-check: contradictions, orphan pages, stale project pages, missing cross-refs, missing concept pages, data gaps
- `lint [product]` — scoped to one product's pages only (saves tokens for focused sessions)

Output: a lint report filed as a synthesis page.

---

## 9. Spec-Driven-AI-Coding Integration

### 9.1 Current command structure (as-built)

Commands live in `~/.ai_global/commands/`. The git-tracked source of truth is `spec-driven-ai-coding/.ai_global/commands/`, synced via `./sync-ai-global.sh`.

The `prompts/` directory in the repo is also a source — some commands originate there. The prompts/ and .ai_global/commands/ have overlapping content; `.ai_global/commands/` is what tools pick up.

| Command | File | Purpose |
|---|---|---|
| `/spec` | `spec.md` | Creates `docs/features/[name]/requirements.md` + README.md |
| `/plan` | `plan.md` | Phase 1 → `design.md`; Phase 2 → `tasks.md` |
| `/executor` | `prompts/executor.md` | Implements one task at a time from `tasks.md` |
| `/audit` | `audit.md` | System health check: spec/design/code alignment |
| `/cleanup` | `cleanup.md` | Dry-run + confirm deletion of dev artifacts (temp files, logs, scratch scripts) |
| `/promote` | `promote.md` | Promotes a project rule to global `~/.ai_global/rules/` |
| `/steering` | `prompts/steering.md` | Creates/updates `.ai-rules/product.md`, `tech.md`, `structure.md` |
| `/wrapup` | **NEW** | Feature lifecycle completion (see §9.5) |

Legacy commands in `.ai_global/commands/_legacy/`: `planner.md`, `executor.md`, `steering.md` (kept for reference; superseded by `/plan`, `/executor` from prompts, `/steering`).

### 9.2 Feature lifecycle (full picture)

```
/spec      → docs/features/[name]/requirements.md + README.md
/plan      → docs/features/[name]/design.md          (Phase 1, stops for review)
/plan      → docs/features/[name]/tasks.md            (Phase 2, stops for review)
/executor  → implements one task at a time
/executor  → ... (repeat until all tasks done)
/wrapup    → extracts docs + cleans temp files + updates WIKI.md + triggers wiki sync
```

Feature docs in `docs/features/[name]/` (requirements.md, design.md, tasks.md) are **never indexed by the wiki** — they are temporary scaffolding. Only the output of `/wrapup` enters the wiki.

### 9.3 `/plan` — wiki-orientation addition

Before Phase 1 (creating `design.md`), the planner reads wiki context:

1. Check if `WIKI.md` exists in the project root. If yes, read it.
2. Read any relevant `docs/architecture/` files for the project.
3. Scan `~/.ai_global/` wiki path (defined in CLAUDE.md) for relevant project and concept pages.

This grounds the design in existing architecture decisions without re-solving solved problems. The plan includes a brief "Wiki context used" note at the top of `design.md`.

The planner does NOT write to the wiki or log to `log.md`.

### 9.4 `/executor` — wrapup reminder

The executor's core behaviour is unchanged. One addition: after marking the LAST task complete (all tasks in `tasks.md` are `[x]`), it adds a reminder:

> "All tasks complete. Run `/wrapup` to extract permanent documentation, clean up feature files, and update the wiki."

### 9.5 `/wrapup` — new command (full specification)

**Purpose:** Bridge from completed feature → permanent docs → wiki. Replaces the implicit expectation that developers will manually update documentation.

**Trigger:** `/wrapup` invoked by Andres after all tasks in `tasks.md` are complete.

**Process:**

**Step 1: Identify the feature**
- Find `docs/features/*/tasks.md` where all tasks are `[x]`
- If multiple, ask Andres which one to wrap up
- Read requirements.md, design.md, tasks.md for that feature

**Step 2: Extract permanent documentation**
- Synthesise a feature summary: what was built, key design decisions, integration points, non-obvious implementation choices
- Write to `docs/architecture/features/[feature-name].md`
- If the feature introduced or changed a schema: update `docs/schemas/` as needed
- If the feature introduced deployment/config changes: note additions to `docs/operations/`

**Step 3: Update `WIKI.md`**
- Append to "Completed features": `YYYY-MM-DD | [feature name] | docs/architecture/features/[feature-name].md`
- Append to "Architecture decisions" for any significant architectural choices made during the feature
- Update "Current status" and "Last updated" date
- Note any cross-project implications (e.g., if a shared API contract changed)

**Step 4: Dry-run cleanup**
- List all files in `docs/features/[feature-name]/` that will be deleted (requirements.md, design.md, tasks.md, README.md, and any other files created for this feature)
- Show file count and total size
- Ask: "Confirm deletion of these [N] files? (yes/no)"

**Step 5: Delete after confirmation**
- Delete `docs/features/[feature-name]/` on confirmation
- `/cleanup` (dev artifacts) is separate and can be run independently; `/wrapup` handles only the feature folder

**Step 6: Remind to sync wiki**
- Output: "Wrapup complete. Run `/wiki sync [project-name]` in Claude to update the wiki."
- (The actual wiki sync runs in Claude, not in Cursor/Antigravity)

**File created by wrapup:** `docs/architecture/features/[feature-name].md`

**Required sections in the feature doc:**
```markdown
# Feature: [Feature Name]
Completed: YYYY-MM-DD

## What it does
[User-facing description]

## Architecture
[How it integrates with the rest of the project]

## Key design decisions
[Decisions made, alternatives considered, rationale]

## Configuration
[Any new env vars, config keys, or deployment changes]

## Known limitations / future work
[Anything deferred or worth noting for future development]
```

### 9.6 Wrapup placement in tools

The `/wrapup` command file is placed in:
- `spec-driven-ai-coding/.ai_global/commands/wrapup.md` (git-tracked source of truth)
- `spec-driven-ai-coding/prompts/wrapup.md` (mirror, for consistency with other prompts)

After adding the file, run `./sync-ai-global.sh` from the spec-driven-ai-coding directory to push it to `~/.ai_global/commands/`. Both Cursor and Antigravity will then pick it up as `/wrapup`.

---

## 10. Cross-Project and Cross-Product Consistency

When a change in one project affects shared concepts, contracts, or entities:

1. `/wrapup` notes cross-project implications in `WIKI.md` of the affected project.
2. The next wiki sync reads those notes and updates all affected wiki pages (project pages, entity pages, decision pages).
3. No automated propagation required — the LLM handles this during sync using shared concept/entity/decision pages as the coordination mechanism.

---

## 11. Page Frontmatter

All wiki pages carry YAML frontmatter for Obsidian Dataview queries:

```yaml
---
type: product | project | source | concept | entity | decision | synthesis | overview
title: "Page title"
product: finfluencer-trade | botastico | rattaproff | null
project: gor_dagster | botastico-api | ...  (null if not project-specific)
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: 3
tags: [dagster, pipeline, python]
---
```

---

## 12. Naming Conventions

- Wiki pages: lowercase, hyphens: `event-sourcing.md`, `botastico-api.md`
- Source summaries: date-prefixed: `2026-03-15-article-slug.md`
- Feature architecture docs: `docs/architecture/features/feature-name.md`
- Log entries: `## [YYYY-MM-DD] type | subject`
- Synthesis pages: descriptive slug, no date: `botastico-stripe-vs-paddle.md`

---

## 13. Tooling

| Tool | Role | Reads |
|---|---|---|
| Claude (Cowork/desktop) | Primary wiki maintainer | `/Users/andreskull/wiki/CLAUDE.md` |
| Cursor | Code development | Project `CLAUDE.md` → references wiki `CLAUDE.md` |
| Antigravity | AI-structured development | Same as Cursor |
| Obsidian | Wiki reader/navigator | Vault at `/Users/andreskull/wiki` |
| Git | Version history | Local only for now |
| Obsidian Web Clipper | Article ingestion | Clips to `raw/articles/` |
| Images | Deferred | — |
| Search tooling | Deferred | Evaluate `qmd` at ~200 pages |

---

## 14. Bootstrap Procedure (First Run)

1. `git init /Users/andreskull/wiki`
2. Create directory structure as per §3
3. Create stub `CLAUDE.md`, `index.md`, `log.md`, `overview.md`
4. Create stub product pages for the three products
5. Create stub project pages for all 11 repos + spec-driven-ai-coding
6. For each project repo: create `WIKI.md` (using structure in §4.1); create `docs/` folder if it doesn't exist
7. Read existing `docs/architecture/` content (where present) to populate project page stubs
8. Log bootstrap to `log.md`
9. Initial `git commit`

**Projects where `docs/` already exists** — to be confirmed during bootstrap; the LLM reads existing docs and indexes them immediately.

---

## 15. Implementation Phases

### Phase 1 — Foundation (next session)
- [ ] Finalise and commit this requirements document
- [ ] `git init /Users/andreskull/wiki`
- [ ] Create directory structure
- [ ] Draft and write `CLAUDE.md`
- [ ] Create `index.md`, `log.md`, `overview.md`

### Phase 2 — Internal project indexing
- [ ] Create `WIKI.md` in each of the 11 project repos + spec-driven-ai-coding
- [ ] Read existing docs for each project to populate project and product wiki pages
- [ ] Create initial concept and entity pages from what's found

### Phase 3 — External source pipeline
- [ ] Ingest first batch of articles/notes into `raw/`
- [ ] Verify ingest workflow and cross-references

### Phase 4 — Spec-driven integration
- [ ] Write `spec-driven-ai-coding/.ai_global/commands/wrapup.md`
- [ ] Write `spec-driven-ai-coding/prompts/wrapup.md`
- [ ] Patch `spec-driven-ai-coding/.ai_global/commands/plan.md` with wiki-orientation step
- [ ] Patch `spec-driven-ai-coding/prompts/executor.md` with wrapup reminder
- [ ] Update `steering.md` to include `WIKI.md` in the list of files it maintains
- [ ] Run `./sync-ai-global.sh` to push changes to `~/.ai_global/`
- [ ] Test the full loop on one feature in one project

### Phase 5 — Tooling and refinement
- [ ] Add Dataview plugin to Obsidian; verify frontmatter queries
- [ ] First lint pass; address health issues
- [ ] Refine `CLAUDE.md` based on experience

---

## 16. Resolved Questions

| Question | Answer |
|---|---|
| All project paths | `/Users/andreskull/[repo-name]` |
| Command mechanism | `.md` files in `~/.ai_global/commands/`; git source in `spec-driven-ai-coding/.ai_global/commands/`; synced via `./sync-ai-global.sh` |
| Active commands | `/spec`, `/plan`, `/executor` (from prompts/), `/audit`, `/cleanup`, `/promote`, `/steering` |
| New command to add | `/wrapup` |
| Do plan/executor log to wiki? | No. Wiki updates only from `/wrapup` + wiki sync |
| Query invocation | Ambient — no special syntax |
| Where to file query answers | LLM decides independently |
| Lint scope | `lint wiki` (full) or `lint [product]` (focused) |
| Obsidian vault | `/Users/andreskull/wiki` |
| Git | Local only |
| Images | Deferred |
| Feature cleanup | Delete after confirmation via `/wrapup`; `/cleanup` handles dev artifacts separately |
| Shared code | None between products |
| Cross-project decisions | LLM updates shared pages during sync |
| Temporary feature docs indexed? | Never — only `docs/architecture/`, `docs/operations/`, `docs/schemas/`, `WIKI.md` |

---

## 17. Remaining Open Items (minor)

1. **Product descriptions** for `botastico` and `rattaproff` — to be filled during bootstrap when repos are accessible.
2. **Botastico repos docs structure** — not yet scanned; check during bootstrap.
3. **Cross-project implications format** in `WIKI.md` — exact format to be established in CLAUDE.md during Phase 1.
4. **`gor_dagster` active features**: 8 feature folders in `docs/features/` (2026-04-25); **Dagster Cloud credit optimization** wrapped to `docs/architecture/features/`. Wiki skips `docs/features/` until `/wrapup`.
