---
name: test
description: >-
  Run comprehensive test suite with pytest, report coverage statistics,
  analyze failures, and attempt automatic fixes. Runs cleanup as a
  prerequisite before testing.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
user-invocable: true
---

Please run comprehensive testing and report results:

**Output Format**: Use clear section headers, timestamps, and structured test result formatting.

**Tool Validation**: First verify that required tools are installed (uv, pytest) and test configuration exists.

**Prerequisites**: First run cleanup command to ensure code quality before testing.

1. **Run all tests**: Execute `uv run pytest` with appropriate timeout handling. If tests timeout, run them in logical sections (e.g., by directory or test file groups) to ensure all tests are executed and results are captured. Always run the complete test suite, even if it requires multiple section runs.
2. **Report results**: Show test coverage summary and details of any failures

**Error Handling**: If tests fail, attempt to fix the issues automatically. If unable to fix, provide detailed failure analysis with specific fix recommendations.

Provide a clear summary of:
- Total tests run
- Tests passed/failed
- Coverage percentage
- Details of any failures with suggestions for fixes
