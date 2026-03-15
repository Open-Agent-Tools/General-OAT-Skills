---
name: check
description: >-
  Run a quick project health check covering git status, recent commits,
  GitHub Actions workflow runs, and package build verification. Use when
  you want a fast overview of project state before starting work.
allowed-tools:
  - Bash
  - Read
  - Grep
  - Glob
user-invocable: true
---

Please perform a quick status check:

**Output Format**: Use clear section headers and timestamps for each check.

**Tool Validation**: First verify that required tools are installed (git, gh, uv) and the project is in a valid git repository.

1. **Git and workflow status (parallel)**: Execute `git status`, `git log --oneline -3`, and `gh run list --limit=3` in parallel to check current state
2. **Package status**: Check if package builds successfully with `uv build`

**Error Handling**: If any command fails, attempt to diagnose and fix the issue. If unable to fix, report the specific error and suggest potential solutions.

Provide a concise summary of the project's current state.
