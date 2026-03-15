---
name: cleanup
description: >-
  Run complete code quality pipeline: ruff linting and formatting, mypy type
  checking, pytest test suite, then update documentation files. Commits and
  pushes all fixes automatically. Use for end-of-session or pre-release cleanup.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
disable-model-invocation: true
user-invocable: true
---

Please perform a complete code cleanup by following these steps:

**Output Format**: Use clear section headers, timestamps for each step, and progress indicators for long-running operations.

1. **Run ruff linting and formatting (parallel)**: Execute `uv run ruff check src --fix` and `uv run ruff format src` (or use `app` if that's the project structure) in parallel to automatically fix linting issues and format code
2. **Run mypy type checking**: Execute `uv run mypy app` or `uv run mypy src` according to the project structure and fix any errors found.
3. **Run pytest via UV**: Execute `uv run pytest` with appropriate timeout handling. If tests timeout, run them in logical sections (e.g., by directory or test file groups) to ensure all tests are executed and results are captured. Always run the complete test suite, even if it requires multiple section runs.
4. **Review and update**: Check TODO.md or *_TODO.md files for recent changes, update checkmarks, and compact older previously completed phases. Update all README.md, CLAUDE.md, and GEMINI.md with recent changes as appropriate.
5. **Commit changes**: Create a detailed commit message summarizing all fixes
6. **Push Commit**: Push to the current branch

**Error Handling**: If any step fails, attempt to fix the issue automatically. If unable to fix, report the specific error details and suggest manual fixes.

Make sure to fix any issues found during each step before proceeding to the next.
