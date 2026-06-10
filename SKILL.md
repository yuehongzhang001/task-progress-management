---
name: task-progress-management
description: Use this for any multi-step coding task, refactor, migration, debugging task, or implementation that may involve more than one file or more than 10 minutes of work. Maintains a persistent markdown task document with goal, plan, progress, decisions, changed files, tests, and handoff.
---

# Task Progress Management

Maintain a single persistent markdown document that tracks a non-trivial task from start to finish, so that progress survives context loss, compaction, or a fresh session, and any agent (or human) can pick up exactly where work stopped.

## When to use

Start or resume a task document whenever a task is likely to involve:
- More than one file, OR
- More than ~10 minutes of work, OR
- A refactor, migration, multi-step debugging investigation, or feature implementation.

For trivial one-line edits or single-question answers, skip this — the overhead isn't worth it.

## Where the document lives

Store the document under the project's `.local/` folder, which must NOT be tracked by git:

```
.local/tasks/<short-task-slug>.md
```

- `<short-task-slug>` is a kebab-case summary of the task, e.g. `migrate-product-images-r2`.
- Use one file per distinct task. Don't pile unrelated tasks into one document.

### Ensure `.local/` is git-ignored (do this first, once)

Before writing the document, confirm `.local/` is ignored:

1. Check whether the project's `.gitignore` already ignores `.local/`.
2. If not, append a `.local/` entry to `.gitignore` (create `.gitignore` if absent).
3. If the repo already tracks anything under `.local/`, untrack it with `git rm -r --cached .local` (keeps the files on disk).

Do this silently as setup — don't make it a big deal.

## Document structure

Use this template. Keep it concise and current — prune stale notes rather than letting it grow unbounded.

```markdown
# Task: <one-line title>

_Last updated: <YYYY-MM-DD HH:MM> · Status: <not-started | in-progress | blocked | done>_

## Goal
<What "done" looks like, in 1–3 sentences. The user-facing outcome, not the steps.>

## Plan
- [ ] Step 1 ...
- [ ] Step 2 ...
- [x] Step 3 ... (done)

## Progress
<Running log, newest at top. Each entry: what was done, what was learned. Brief.>
- <YYYY-MM-DD HH:MM> — ...

## Decisions
<Choices made and WHY. Trade-offs, rejected alternatives, constraints discovered.>
- ...

## Changed files
<Files created/modified/deleted and a one-line note on each.>
- `path/to/file` — ...

## Tests
<How correctness is verified: commands to run, what passed/failed, what's untested.>
- `command` — <result>

## Handoff / Next steps
<The single most important section for resuming. What to do next, open questions,
anything a fresh agent needs to know that isn't obvious from the code.>
- ...
```

## Workflow

1. **At task start** — Create the document (after ensuring `.local/` is ignored). Fill in Goal and an initial Plan. Set status to `in-progress`.
2. **As you work** — Keep the document live, not as an afterthought:
   - Check off Plan items as completed; add new ones as the plan evolves.
   - Append to Progress when you finish a meaningful chunk or learn something.
   - Record any non-obvious choice in Decisions with its rationale.
   - Update Changed files and Tests as they change.
   - Bump `Last updated`.
3. **Before risky or long operations** (and before you expect context to compact) — Update Handoff / Next steps so resumption is clean.
4. **At task end** — Set status to `done`, ensure Tests reflects final verification, and leave Handoff empty or noting follow-ups.

## Resuming an existing task

When asked to continue work, first look for an existing document in `.local/tasks/`. If one matches, read it fully before acting — treat Handoff / Next steps and Decisions as the source of truth for where things stand. Verify any file/function it references still exists before relying on it, then continue and keep updating the same file.

## Principles

- **One source of truth.** Don't scatter status across chat and code comments — the document is canonical.
- **Write for a cold reader.** Assume whoever resumes has zero memory of this session. Include paths and rationale, not just "fixed the bug."
- **Keep it tight.** Update in place; delete obsolete notes. A 2-page living doc beats a 20-page transcript.
- **Never commit it.** The document is local scratch state under git-ignored `.local/`.
