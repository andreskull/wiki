# Patch: plan.md — Wiki Orientation Step

Add the following section immediately after the "## Prerequisites" section and before "## Process" in `plan.md` (and in `.ai_global/commands/plan.md`).

---

## INSERT AFTER "## Prerequisites" — BEFORE "## Process"

```markdown
## Wiki Context (Read Before Planning)

Before generating the design, orient yourself in the existing project knowledge:

1. **Read `WIKI.md`** — check if `WIKI.md` exists in the project root. If it does, read it fully. Pay attention to:
   - "Architecture decisions" — avoid re-solving already-solved problems
   - "Completed features" — understand what's already been built
   - "Technology stack" — confirm your design uses established patterns

2. **Read `docs/architecture/README.md`** — if it exists, read it for overall architecture context

3. **Check for related features** — scan `docs/architecture/features/` for any previously completed features that the current feature builds on or interacts with

4. **Note what you used** — at the top of `design.md`, include a brief section:
   ```markdown
   ## Wiki Context Used
   - Read WIKI.md: [yes/no — if yes, note 1-2 key constraints it informed]
   - Related features: [list any from docs/architecture/features/ that were relevant]
   ```

If `WIKI.md` does not exist yet, proceed without it and note "WIKI.md not present" in the Wiki Context Used section.
```

---

## WHERE TO INSERT IN plan.md

Find this block:
```
## Prerequisites

Before running this command:
- Ensure `docs/features/feature-name/requirements.md` exists
- Review the requirements to understand the feature scope
```

Insert the new "## Wiki Context" section immediately after that block, before "## Process".
