# Wiki Operating Manual

This is the schema for Andres's personal LLM wiki. Read this file at the start of every session before doing anything else. It tells you how the wiki is structured, what the conventions are, and exactly what to do for each operation.

---

## Session Startup

Every session begins with these four reads in order:

1. This file (`CLAUDE.md`)
2. `index.md` — current catalog of all wiki pages
3. `log.md` tail — last 10 entries to understand what happened recently (`grep "^## \[" log.md | tail -10`)
4. `wiki/overview.md` — high-level synthesis of everything

After these four reads you are oriented and ready to work.

---

## Product and Project Map

### Products

| Product | Description | Repos |
|---|---|---|
| **finfluencer.trade** | Financial influencer tracking and accountability platform | `gor_dagster`, `gor-blog`, `finfluencer-tracker` |
| **botastico** | (TBD — repos not yet indexed) | `botastico`, `botastico-api`, `botastico-script`, `botastico-portal`, `botastico-stripe`, `botastico-script-inserter`, `botastico-opiq-case-study` |
| **rattaproff** | WooCommerce multi-store automation and Google Indexing management | `rattaproff` |

### Project paths

All repos at `/Users/andreskull/[repo-name]`.

| Repo | Product | Docs state |
|---|---|---|
| `gor_dagster` | finfluencer.trade | Rich — architecture/, operations/, schemas/, analytics/, features/ |
| `gor-blog` | finfluencer.trade | MkDocs site — docs/ IS the blog; posts at docs/blog/posts/ |
| `finfluencer-tracker` | finfluencer.trade | Minimal — one doc file |
| `rattaproff` | rattaproff | Scaffold — architecture/, operations/, schemas/ exist but sparse |
| `botastico*` | botastico | Not yet indexed |
| `spec-driven-ai-coding` | — | Methodology project, not a product repo |

---

## Directory Structure

```
/Users/andreskull/wiki/
├── CLAUDE.md          ← this file
├── index.md           ← master content catalog
├── log.md             ← append-only operation log
├── raw/               ← external sources (immutable after ingestion)
│   ├── articles/
│   ├── papers/
│   ├── notes/
│   └── data/
└── wiki/              ← LLM-generated pages (you own this layer)
    ├── overview.md
    ├── products/
    ├── projects/
    ├── concepts/
    ├── entities/
    ├── decisions/
    └── synthesis/
```

**Rule:** You write everything in `wiki/`. You read from `raw/` but never modify it. `index.md` and `log.md` are at the root, updated by you on every operation.

---

## What Gets Indexed from Project Repos

For each project repo at `/Users/andreskull/[repo]`:

**Index:** `WIKI.md` (root) + everything in `docs/` **except** `docs/features/`

**Never index:** `docs/features/` (temporary spec-driven work — indexed only after `/wrapup`)

**Special case — `gor-blog`:** `docs/` is the MkDocs site. Index `docs/blog/posts/` as published content outputs. No architecture docs separate from site content.

---

## Page Types and Required Sections

### Product page (`wiki/products/`)
- Frontmatter: `type: product`
- Sections: What it does, target users, component repos (with one-liner each), overall architecture summary, current status, key cross-repo decisions, links to all project pages

### Project page (`wiki/projects/`)
- Frontmatter: `type: project`
- Sections: Which product, purpose and role, tech stack (key items only), architecture summary, key decisions, completed features (with links to `docs/architecture/features/`), current status, link to `WIKI.md`, related wiki pages

### Source summary (`wiki/sources/`)
- Frontmatter: `type: source`
- Sections: Title, URL, date ingested, key takeaways (3-7 bullets), connections to existing projects/concepts, contradictions or updates to existing claims

### Concept page (`wiki/concepts/`)
- Frontmatter: `type: concept`
- Sections: Definition, relevance to Andres's work, which projects use it, which sources discuss it, related concepts

### Entity page (`wiki/entities/`)
- Frontmatter: `type: entity`
- Sections: What it is, how it's used, which projects depend on it, related entities

### Decision page (`wiki/decisions/`)
- Frontmatter: `type: decision`
- Sections: Context, options considered, decision made, date, consequences, affected projects/repos

### Synthesis page (`wiki/synthesis/`)
- Frontmatter: `type: synthesis`
- Sections: as appropriate for the content — you decide the structure
- File when: the answer to a query is substantive enough to be worth keeping. Use your judgment. Don't ask.

### Overview (`wiki/overview.md`)
- Frontmatter: `type: overview`
- Sections: Products and current focus, active development threads, recent learnings, open questions
- Update: during lint passes, or when the landscape shifts meaningfully

---

## Frontmatter Schema

Every wiki page starts with YAML frontmatter:

```yaml
---
type: product | project | source | concept | entity | decision | synthesis | overview
title: "Page title"
product: finfluencer-trade | botastico | rattaproff | null
project: gor_dagster | gor-blog | finfluencer-tracker | rattaproff | null
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [dagster, pipeline, python]
---
```

---

## Cross-Reference Conventions

- Internal wiki links: Obsidian wikilink syntax `[[projects/gor_dagster]]`
- Links to project files: markdown link with absolute path `[core-data-model.md](file:///Users/andreskull/gor_dagster/docs/architecture/core-data-model.md)`
- External URLs: cited in a "Sources" section at the bottom of the page
- Every page has a "Related pages" section listing 3-5 connected wiki pages

---

## index.md Format

One entry per wiki page, organised by section (Products, Projects, Sources, Concepts, Entities, Decisions, Synthesis). Format:

```
- [[products/finfluencer-trade]] — Financial influencer tracking platform (3 repos). Updated 2026-04-06.
- [[projects/gor_dagster]] — Dagster pipeline for finfluencer.trade data ingestion and analysis. Updated 2026-04-06.
```

Update `index.md` on every write operation. Read it first on every query.

---

## log.md Format

Append-only. Every entry starts with a parseable prefix:

```
## [YYYY-MM-DD] ingest | Source title
## [YYYY-MM-DD] sync | project-or-product-name
## [YYYY-MM-DD] wrapup | project-name | Feature name
## [YYYY-MM-DD] lint | full
## [YYYY-MM-DD] lint | finfluencer-trade
## [YYYY-MM-DD] query-filed | Synthesis page title
## [YYYY-MM-DD] bootstrap | Initial wiki created
```

Grep pattern for recent entries: `grep "^## \[" log.md | tail -10`

---

## Operations

### Ingest (external source)
Triggered: Andres drops a file in `raw/` and mentions it.

1. Read `index.md`
2. Read the source; discuss key takeaways with Andres
3. Create `wiki/sources/YYYY-MM-DD-slug.md`
4. Update all relevant product, project, concept, entity pages; flag contradictions
5. Update `index.md`
6. Append to `log.md`: `## [date] ingest | [title]`

### Sync (project or product)
Triggered: manual, scoped.

1. Read project `WIKI.md` + relevant `docs/` files (skip `docs/features/`)
2. Update corresponding wiki page(s)
3. Update affected concept, entity, decision pages
4. Update `index.md`
5. Append to `log.md`: `## [date] sync | [scope]`

### Queries
No command needed — wiki is ambient context. Read `index.md`, find relevant pages, answer with citations. File substantial answers as synthesis pages on your own judgment without being asked.

### Lint
- `lint wiki` — full health check: contradictions, orphan pages, stale project pages, missing cross-references, missing concept pages
- `lint [product]` — scoped to one product's pages only

Output: synthesis page with prioritised action list. Append to log.

---

## Wrapup (triggered from project tool, not here)

When a developer runs `/wrapup` in Cursor or Antigravity after completing a feature, it:
1. Writes `docs/architecture/features/[feature-name].md` in the project repo
2. Updates the project's `WIKI.md`
3. Deletes `docs/features/[feature-name]/` after confirmation

After wrapup, Andres runs a wiki sync here: `sync [project-name]`
That sync reads the new `docs/architecture/features/` file and `WIKI.md` and updates the wiki project page and related pages.

---

## Naming Conventions

- Wiki pages: lowercase, hyphens: `event-sourcing.md`, `actionable-signal.md`
- Source summaries: date-prefixed: `2026-03-15-wallstreetbets-paper.md`
- Synthesis pages: descriptive slug, no date: `stt-provider-comparison.md`
- Log entries: always `## [YYYY-MM-DD] type | subject`

---

## Tools in Use

- **Claude (Cowork/desktop)** — primary wiki maintainer (you)
- **Cursor** — code development, uses `/spec`, `/plan`, `/executor`, `/wrapup`
- **Antigravity** — AI-structured development, same command set
- **Obsidian** — wiki reader (vault root = `/Users/andreskull/wiki`)
- **Git** — local only for now (`/Users/andreskull/wiki`)
