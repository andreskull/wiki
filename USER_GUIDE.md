# Wiki User Guide

This is your guide to working with the wiki. The wiki is a living knowledge base maintained by Claude — you read it, ask questions against it, and tell Claude when to update it.

---

## What the wiki is

A set of markdown files at `/Users/andreskull/wiki/` that Claude keeps up to date. It captures what you know about your projects so you don't have to re-explain context every session. Open it in Obsidian (vault root = `/Users/andreskull/wiki`) to browse and search.

The wiki has two layers:
- **`wiki/`** — LLM-generated pages (products, projects, concepts, entities, decisions, synthesis)
- **`raw/`** — external sources you've dropped in (articles, papers, notes) — immutable after ingestion

---

## The four things you do with the wiki

### 1. Ask questions

Just ask in Claude. No command needed.

> "What's the current speaker attribution pipeline architecture?"
> "Which LLM config IDs are used for facts extraction?"
> "What does the proof-segment speaker gate do?"

Claude reads `index.md`, finds the relevant pages, and answers with citations. If the answer is substantive enough to keep, Claude files it as a synthesis page automatically.

### 2. Add an external source (article, paper, note)

Drop the file in `raw/articles/`, `raw/papers/`, or `raw/notes/`, then tell Claude:

> "I added an article to raw/articles/ — please ingest it."

Claude will:
1. Read and discuss the key takeaways with you
2. Create a source summary page in `wiki/sources/`
3. Update any relevant product/project/concept pages
4. Update `index.md` and `log.md`

### 3. Sync a project after completing work

After you finish a feature (via `/wrapup` in Cursor or Antigravity), tell Claude:

> "Sync gor_dagster to the wiki."

Claude reads the updated `WIKI.md` and `docs/architecture/` in that repo and updates the wiki project page, related concept pages, and cross-references.

You can also sync at any time if a repo's architecture has changed:
> "Sync rattaproff to the wiki."

### 4. Run a lint pass

Ask Claude to check the wiki's health:

> "Lint the wiki." — full check across all pages
> "Lint finfluencer.trade." — scoped to one product's pages

Claude produces a prioritised list of: contradictions, orphan pages, stale content, missing cross-references, and missing concept pages. Results are saved as a synthesis page.

---

## The development workflow

When you're building a feature in Cursor or Antigravity, the wiki stays out of the way. The flow is:

```
/spec       → docs/features/[name]/requirements.md
/plan       → docs/features/[name]/design.md  (Phase 1)
/plan       → docs/features/[name]/tasks.md   (Phase 2)
/executor   → implement, one task at a time
/wrapup     → permanent docs + WIKI.md update + cleanup
```

Then tell Claude to sync:
```
"Sync [project] to the wiki."
```

The wiki never touches `docs/features/` — those are temporary and get deleted by `/wrapup`.

---

## Quick reference: what to say to Claude

| You want to... | Say... |
|---|---|
| Ask a question | Just ask it |
| Ingest an article you added to raw/ | "Ingest the article I added to raw/articles/" |
| Update wiki after a wrapup | "Sync gor_dagster to the wiki" |
| Update wiki after any repo change | "Sync [project name] to the wiki" |
| Check for stale or broken content | "Lint the wiki" |
| Check one product only | "Lint finfluencer.trade" |
| Add a new wiki page manually | "Create a concept page for [topic]" |
| Find out what's in the wiki | "What pages does the wiki have on speaker attribution?" |

---

## What's in the wiki right now

**Products:** finfluencer.trade, rattaproff, botastico (stub)

**Projects:** gor_dagster, gor-blog, finfluencer-tracker, rattaproff, spec-driven-ai-coding

**Concepts:** speaker-attribution, actionable-signal, llm-config-registry, spec-driven-development

**Entities:** bigquery, dagster

Browse the full catalog in `index.md` or open the vault in Obsidian.

---

## Files you should know about

| File | What it is |
|---|---|
| `CLAUDE.md` | Operating manual for Claude — how the wiki works internally |
| `USER_GUIDE.md` | This file |
| `index.md` | Master catalog of all wiki pages |
| `log.md` | Append-only history of every wiki operation |
| `wiki/overview.md` | High-level synthesis of everything — good starting point |
| `requirements.md` | Original design spec for the wiki |

---

## Tips

**Obsidian:** The wiki uses `[[wikilink]]` syntax. Open `/Users/andreskull/wiki` as a vault and the graph view shows how pages connect.

**Git:** The wiki is a local git repo. Commit when you want a snapshot — Claude doesn't commit automatically.

**Keep WIKI.md files current:** After significant architecture changes in a project repo, update that repo's `WIKI.md` (Claude can do this) before running a sync. The sync reads `WIKI.md` as its entry point.

**Don't edit `raw/`:** Files in `raw/` are immutable after ingestion. If a source changes your understanding of something, tell Claude — it will update the relevant wiki pages.
