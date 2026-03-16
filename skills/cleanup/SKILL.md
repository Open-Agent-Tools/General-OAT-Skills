---
name: cleanup
description: >-
  Runs a complete code quality pipeline: ruff linting and formatting, mypy type
  checking, and pytest test suite. Use for end-of-session quality checks,
  pre-release validation, or when the user asks to lint, format, or clean up code.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
disable-model-invocation: true
user-invocable: true
---

Perform a complete code cleanup by following these steps:

**Output Format**: Use clear section headers, timestamps for each step, and progress indicators for long-running operations.

**Tool Validation**: First verify that required tools are installed (uv, ruff, mypy, pytest) and identify the project source layout (src/ or app/).

### Checklist

- [ ] Ruff linting and formatting
- [ ] Mypy type checking
- [ ] Pytest suite
- [ ] Summary

1. **Run ruff linting and formatting (parallel)**: Execute `uv run ruff check src --fix` and `uv run ruff format src` (or use `app` if that's the project structure) in parallel to automatically fix linting issues and format code. Re-run to verify all issues resolved.
2. **Run mypy type checking**: Execute `uv run mypy src` (or `app`) according to the project structure and fix any errors found. Re-run to verify fixes.
3. **Run pytest via UV**: Execute `uv run pytest` with appropriate timeout handling. If tests timeout, run them in logical sections (e.g., by directory or test file groups) to ensure all tests are executed and results are captured.

**Error Handling**: For each step, run the tool, fix issues found, then re-run to confirm the fix. If unable to fix after two attempts, report the specific error details and suggest manual fixes.

Update the checklist above as each step completes. Provide a final summary of all changes made.
