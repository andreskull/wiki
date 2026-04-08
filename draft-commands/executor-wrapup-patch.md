# Patch: executor.md — Wrapup Reminder

Add a wrapup reminder at the end of Step 6 in `executor.md` (the "Update State & Report" step), specifically in the "all tasks complete" case.

---

## WHERE TO INSERT

In the `# **IMPORTANT EXECUTION INSTRUCTIONS**` section, under `## **Normal Mode (Default)**`, find:

```
- Once you complete the requested task (including tests), stop and let the user review. DO NOT just proceed to the next task in the list
```

After that line, add:

```markdown
- **If this was the LAST task** (all remaining tasks in `tasks.md` are now `[x]`): after stopping, add this message:
  > "🎉 All tasks complete. Run `/wrapup` to extract permanent documentation, clean up the feature folder, and update WIKI.md so the wiki can be synced."
```

---

## ALSO INSERT in the General Rules section

Find:
```
# **General Rules**
- Never anticipate or perform actions from future steps, even if you believe it is more efficient.
```

After the General Rules heading, add:

```markdown
- After marking the final task complete, always remind the user to run `/wrapup` before closing the feature.
```

---

## Note on context reading

The executor already reads `docs/architecture/` before executing tasks (per the "Documentation Context" section). No change needed there — that section already provides the right wiki-sourced knowledge path. The executor reads `WIKI.md` implicitly via `@docs/` context; no explicit WIKI.md read step is required in the executor itself since WIKI.md is in the project root and not in docs/. If desired in the future, add `WIKI.md` to the executor's context reading list under "Documentation Context".
