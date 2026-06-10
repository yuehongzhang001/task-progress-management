---
name: task-progress-management
description: Use this for any multi-step coding task, refactor, migration, debugging task, or implementation that may involve more than one file or more than 10 minutes of work. Maintains a persistent markdown task document with goal, plan, progress, decisions, changed files, tests, and handoff.
---

# Task Progress Management

Maintain a single persistent markdown document that tracks a non-trivial task, so progress survives context loss, compaction, or a fresh session, and any agent (or human) can pick up exactly where work stopped.

## When to use

Start or resume a task document whenever a task is likely to involve more than one file, more than ~10 minutes of work, or is a refactor, migration, multi-step debugging investigation, or feature implementation. Skip it for trivial one-line edits or single-question answers.

## Where the document lives

Store it under the project's `.local/` folder, which must NOT be tracked by git:

```
.local/tasks/<short-task-slug>.md
```

One file per distinct task. Before writing it the first time, make sure `.local/` is git-ignored: check `.gitignore`, append a `.local/` entry if missing (create the file if absent), and if anything under `.local/` is already tracked, untrack it with `git rm -r --cached .local`. Do this quietly as setup.

## What the document MUST contain

Only three things are required. Everything else is up to you — add whatever sections best fit this particular task (decisions, changed files, tests, open questions, notes, links, etc.) and structure them however serves the work. Don't pad the document with empty headings.

1. **The task** — what we're trying to accomplish, in a sentence or few. What "done" looks like.
2. **A checklist of steps** — the task broken into concrete steps, with completed ones clearly marked (`- [x]`) and remaining ones unchecked (`- [ ]`). Update it as the plan evolves — add, split, or drop steps as you learn.
3. **The next step** — call out explicitly which checklist item is being worked on next, so a cold reader knows exactly where to resume without inferring it.

A minimal valid document is just:

```markdown
# <task title>

<What we're accomplishing / what done looks like.>

## Steps
- [x] ...
- [x] ...
- [ ] ...  ← next
- [ ] ...

**Next:** <the item being picked up next, and anything needed to start it.>
```

Beyond those three, use your judgment on what's worth recording — capture the non-obvious (a decision and why, a gotcha discovered, how to verify) and skip the obvious.

## Workflow

- **At task start** — create the document (after ensuring `.local/` is ignored): write the task, an initial checklist, and the next step.
- **As you work** — keep it live, not as an afterthought. Check off steps as they're done, revise the checklist as the plan changes, and keep the **Next** pointer current. Record anything non-obvious you'd want a fresh reader to know.
- **Before risky or long operations, and before context is likely to compact** — make sure the checklist and Next pointer reflect reality so resumption is clean.
- **At task end** — all steps checked; note final verification if relevant.

## Resuming

When asked to continue, first look for an existing document in `.local/tasks/`. If one matches, read it fully before acting — the checklist and **Next** pointer are the source of truth for where things stand. Verify any file or symbol it references still exists before relying on it, then continue and keep updating the same file.

## Principles

- **One source of truth.** Don't scatter status across chat and code comments — the document is canonical.
- **Write for a cold reader.** Assume whoever resumes has zero memory of this session. Include paths and rationale, not just "fixed the bug."
- **Keep it tight.** Update in place, prune stale notes. A short living doc beats a long transcript.
- **Never commit it.** It's local scratch state under git-ignored `.local/`.
