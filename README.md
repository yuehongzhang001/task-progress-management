# task-progress-management

A [Claude Code](https://claude.com/claude-code) skill that maintains a persistent markdown task document for any non-trivial coding work.

Use it for any multi-step coding task, refactor, migration, debugging task, or implementation that may involve more than one file or more than ~10 minutes of work. It keeps a single living document with **goal, plan, progress, decisions, changed files, tests, and handoff**, so progress survives context loss, compaction, or a fresh session — and any agent or human can pick up exactly where work stopped.

The document lives under the project's git-ignored `.local/tasks/<slug>.md`, so it never gets committed.

## Install

Drop the skill into your skills directory:

```bash
# Project-level
mkdir -p .claude/skills/task-progress-management
curl -sL https://raw.githubusercontent.com/yuehongzhang001/task-progress-management/main/SKILL.md \
  -o .claude/skills/task-progress-management/SKILL.md

# Or global (all projects)
mkdir -p ~/.claude/skills/task-progress-management
curl -sL https://raw.githubusercontent.com/yuehongzhang001/task-progress-management/main/SKILL.md \
  -o ~/.claude/skills/task-progress-management/SKILL.md
```

Then restart Claude Code (or start a new session) and the skill becomes available.

## What it does

See [SKILL.md](SKILL.md) for the full document template and workflow.
