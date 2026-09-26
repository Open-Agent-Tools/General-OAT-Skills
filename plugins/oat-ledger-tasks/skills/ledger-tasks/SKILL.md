---
name: ledger-tasks
description: >-
  Task and dependency management for coding agents via YYLO Ledger: Kanban
  board and task state stored as hash-chained Markdown inside the repository.
  Use for creating, listing, searching, and updating tasks, dependency
  graphs (deps/ready/order), and typed task lifecycle flows.
allowed-tools:
  - Bash
  - Read
  - Grep
argument-hint: "[task command or question]"
user-invocable: true
---

Manage tasks and dependencies with YYLO Ledger, a Git-native task ledger for
coding-agent projects. Task state lives in plain Markdown under `.juno_task/`
and is hash-chained, so board history is reviewable in normal commits.

**Prerequisites**: The `yy` CLI from the [YYLO Ledger](https://github.com/yylo-dev/yylo-ledger)
project (`npm install -g @yylo/cli`). Preflight with `yy ledger --version` and
`yy ledger --help`; installed command help is authoritative for the selected
runtime. If a required command group is absent, stop and request a Ledger
upgrade rather than guessing flags.

**Tool Validation**: Verify `yy ledger --version` runs before using any
command below. Never edit Ledger store files by hand; go through the CLI so
the hash chain and receipts stay intact.

### Task commands

```bash
# Create a task (status: backlog|todo|in_progress|done)
yy ledger create "Task description" --status backlog --tags feature,backend

# List with summary stats
yy ledger list --status todo,in_progress --limit 10

# Search by criteria (--status --tag --body --response --commit --open --recent)
yy ledger search --tag backend --open --limit 10

# Universal read: full task details incl. resolved dependencies
yy ledger get TASK_ID

# Update status with a required response message
yy ledger mark in_progress --id TASK_ID --response "Starting work"
yy ledger mark done --id TASK_ID --response "Done: implemented X, tested Y" --commit abc123

# Modify fields / attach a commit
yy ledger update TASK_ID --tags backend,urgent --commit abc123def

# Soft delete (preserves data)
yy ledger archive TASK_ID
```

`mark` requires `--id` and `--response`; always document what was done and how
it was tested. `--commit` links the git history when marking done.

### Dependencies and planning

```bash
# View blockers, dependents, priority score
yy ledger deps TASK_ID

# TASK_ID cannot start until blockers are done
yy ledger deps add --id TASK_ID --blocked-by BLOCKER1 BLOCKER2
yy ledger deps remove --id TASK_ID --blocked-by BLOCKER1

# Unblocked tasks (backlog/todo/in_progress with all blockers done)
yy ledger ready --tag backend --limit 5

# Topological order for safe parallel execution
yy ledger order --scores
```

Dependencies and related tasks can also be declared inline in task bodies and
are parsed automatically on create/update:

```
[blocked_by]TASK_ID[/blocked_by]        — blocked by TASK_ID
[blocked_by]ID1, ID2[/blocked_by]       — blocked by multiple
[task_id]RELATED_ID[/task_id]           — cross-reference a related task
```

### Agent task lifecycle

For managed delivery (dedicated worktree per task, validation-gated finish):

```bash
yy task start TASK_ID     # hydrate a feature worktree and begin work
yy task finish TASK_ID    # enforce admission + validation on close
yy merge status TASK_ID   # observe merge state
yy merge land TASK_ID     # target owner lands and records the Git result
```

Concurrent features are supported: each started task gets its own worktree;
`yy merge` serializes only target updates.

### Best practices

1. Create tasks small enough to finish in one iteration without filling the context window
2. Status flow: backlog → todo → in_progress → done (archive for abandoned work)
3. Always pass `--response` with `mark`; attach `--commit` when marking done
4. Run `ready` before starting work and `order --scores` to plan parallel pipelines
5. Read task state before mutating; keep mutation receipts where offered
6. Report the actual Record ID and slug returned by the CLI — never invent one from the title

### Error handling

If a command or flag is missing in the installed runtime, fail closed: report
the gap and the version, and ask for a Ledger upgrade. Routing or branch
errors never fall back silently. Cross-project access is opt-in via
`kanbanRegistry.enabled` in `.juno_task/config.json` and explicit
`allowedProjects`; without it, commands stay inside the current project.

### Source

Adapted from the [YYLO Skills](https://github.com/yylo-dev/yylo-skills) pack
(MIT, upstream); contributed under this repository's Apache-2.0 license.
CLI and runtime: [yylo-dev/yylo-ledger](https://github.com/yylo-dev/yylo-ledger).
