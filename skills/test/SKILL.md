---
name: test
description: >-
  Runs the comprehensive test suite with pytest, reports coverage statistics,
  analyzes failures, and attempts automatic fixes. Use when the user wants to
  run tests, check coverage, or validate that code changes pass the test suite.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
user-invocable: true
---

Run comprehensive testing and report results:

**Output Format**: Use clear section headers, timestamps, and structured test result formatting.

**Tool Validation**: First verify that required tools are installed (uv, pytest) and test configuration exists.

1. **Run all tests**: Execute `uv run pytest` with appropriate timeout handling. If tests timeout, run them in logical sections (e.g., by directory or test file groups) to ensure all tests are executed and results are captured. Always run the complete test suite, even if it requires multiple section runs.
2. **Analyze failures**: If tests fail, attempt to fix the issues automatically. Re-run to confirm fixes. If unable to fix after two attempts, provide detailed failure analysis with specific fix recommendations.
3. **Report results**: Show test coverage summary and details of any failures

Provide a clear summary of:
- Total tests run
- Tests passed/failed
- Coverage percentage
- Details of any failures with suggestions for fixes
