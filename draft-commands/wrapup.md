# Wrapup Command

Finalise a completed feature: extract permanent documentation, update `WIKI.md`, and clean up temporary feature files. This command runs after all tasks in `tasks.md` are marked complete.

## Purpose

The development lifecycle produces temporary files (`requirements.md`, `design.md`, `tasks.md`) that guide implementation but should not live in the codebase forever. Wrapup is the transition from "feature in progress" to "feature shipped and documented." It:

1. Extracts essential information into permanent `docs/architecture/features/` documentation
2. Updates the project's `WIKI.md` so the wiki can be synced
3. Deletes the temporary feature folder after confirmation

## Prerequisites

- All tasks in `docs/features/[feature-name]/tasks.md` must be `[x]` (complete)
- If any tasks are incomplete, STOP and inform the user

## Process

### Step 1: Identify the feature

- Scan `docs/features/*/tasks.md` for a file where all tasks are `[x]`
- If none found: inform user "No completed features found. All tasks must be marked [x] before running /wrapup."
- If multiple completed features exist: list them and ask which one to wrap up
- Read the identified `requirements.md`, `design.md`, and `tasks.md` fully

### Step 2: Extract permanent documentation

Create `docs/architecture/features/[feature-name].md` using this structure:

```markdown
# Feature: [Feature Name]
Completed: YYYY-MM-DD

## What it does
[User-facing description: what problem it solves, how it's used]

## Architecture
[How this feature integrates with the rest of the project — components touched, data flow, key interfaces]

## Key design decisions
[Decisions made during design, alternatives that were considered, rationale for choices made]

## Configuration
[New environment variables, config keys, feature flags, or deployment changes introduced]

## Known limitations / future work
[Anything deliberately deferred, known constraints, or ideas for future improvement]
```

Guidelines for extraction:
- Focus on **what future developers (and the LLM) need to know**, not what you did step by step
- Capture decisions that aren't obvious from reading the code
- If `design.md` contains architecture diagrams or data models worth keeping, include them
- Keep it concise — this is reference documentation, not a story

If the feature introduced schema changes, note the update needed in `docs/schemas/`. If it introduced deployment or config changes, note the update needed in `docs/operations/`. **Do not update those files automatically** — note them as follow-up tasks for the user.

### Step 3: Update `WIKI.md`

In the project root `WIKI.md`:

1. **Append** to "Completed features":
   ```
   YYYY-MM-DD | [Feature Name] | docs/architecture/features/[feature-name].md
   ```

2. **Append** to "Architecture decisions" for any significant decisions made during this feature (decisions that affect how future features should be built):
   ```
   YYYY-MM-DD | [decision summary] | [rationale in one sentence]
   ```

3. **Update** "Current status" with a one-line description of the project's current state

4. **Update** "Last updated" date

5. **Note cross-project implications** (if the feature changed something that other projects depend on — e.g., an API contract, a shared data schema, a service interface):
   ```
   CROSS-PROJECT: [date] | [what changed] | affects: [list of other repos]
   ```

### Step 4: Dry-run cleanup

List all files in `docs/features/[feature-name]/`:
- Show full file paths
- Show total file count and estimated size

Present to user:
```
Ready to delete the following [N] files from docs/features/[feature-name]/:
  - docs/features/[feature-name]/requirements.md
  - docs/features/[feature-name]/design.md
  - docs/features/[feature-name]/tasks.md
  - docs/features/[feature-name]/README.md
  [any other files]

Permanent documentation has been written to:
  docs/architecture/features/[feature-name].md

Confirm deletion? (yes/no)
```

**NEVER delete without explicit user confirmation.**

### Step 5: Delete after confirmation

On "yes": delete the `docs/features/[feature-name]/` directory and all its contents.
On "no": stop. The permanent doc and WIKI.md updates remain. User can delete manually later.

### Step 6: Remind to sync wiki

Output this message after completion:

```
✅ Wrapup complete for [Feature Name].

Files created:
  docs/architecture/features/[feature-name].md

WIKI.md updated:
  [project-root]/WIKI.md

Next step: Run a wiki sync in Claude:
  "sync [project-name] to wiki"

This will update the wiki project page and cross-references.
```

## Safety Rules

- **NEVER delete files without explicit user confirmation** (same principle as `/cleanup`)
- **NEVER run if tasks are incomplete** — check all tasks are `[x]` before starting
- **NEVER overwrite existing `docs/architecture/features/` files without warning** — if a file already exists, show the existing content and ask whether to overwrite or append
- Do NOT commit changes — leave that to the user

## Integration

- Runs after `/executor` completes all tasks
- Output feeds into the wiki sync operation (run manually in Claude)
- `/cleanup` is separate — it handles dev artifact files (scratch scripts, debug logs, temp files). Run it independently if needed.
- Works alongside `/audit` — run `/audit` before `/wrapup` if you want to verify code quality first

## What This Command Does NOT Do

- Does NOT run tests (tests should already be passing from `/executor`)
- Does NOT commit to git
- Does NOT update the wiki directly (wiki sync is a separate Claude operation)
- Does NOT touch files outside `docs/features/[feature-name]/` and `docs/architecture/features/` and `WIKI.md`
