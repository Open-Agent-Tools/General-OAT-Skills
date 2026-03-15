---
name: review
description: >-
  Review code for quality improvements, performance optimizations, error
  handling, and language-specific best practices. Provide actionable
  suggestions with automatic fixes for common issues.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
context: fork
user-invocable: true
argument-hint: "[file-or-directory-to-review]"
---

Please review the following code and provide suggestions for:

**Output Format**: Use clear section headers and structured formatting for each type of suggestion.

**Tool Validation**: First verify that the code files exist and are accessible for analysis.

1. Code quality improvements
2. Performance optimizations
3. Better error handling
4. Following language-specific best practices

**Error Handling**: If code cannot be analyzed due to syntax errors or missing dependencies, attempt to identify and fix the issues automatically. If unable to fix, report the specific problems.

$ARGUMENTS
