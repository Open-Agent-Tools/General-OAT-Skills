---
name: load
description: >-
  Loads all markdown files from the current directory or a specified directory
  into context for analysis and reference. Non-recursive, loads only top-level
  .md files. Use when the user wants to read all docs in a folder, review
  documentation, or bring markdown content into the conversation.
allowed-tools:
  - Glob
  - Read
  - Bash
context: fork
user-invocable: true
argument-hint: "[directory-path]"
---

Please load all markdown (.md) files from the current directory or specified directory into context for analysis and reference.

**Important**: Only load files directly in the specified directory, NOT in any subdirectories.

**Process**:
1. Use glob to find all .md files in the target directory (non-recursive)
2. Read each file found to bring it into context
3. Provide a brief summary of what files were loaded

**Directory Selection**:
- If no directory is specified in arguments, use current directory
- If directory is specified, use that path
- Validate directory exists before processing

**Output Format**:
- List each file loaded with its path
- Provide brief description of total files processed
- Note any files that couldn't be read

$ARGUMENTS
