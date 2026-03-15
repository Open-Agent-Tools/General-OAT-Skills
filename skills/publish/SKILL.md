---
name: publish
description: >-
  Prepare and trigger a new release: run cleanup, validate semantic versioning,
  update pyproject.toml and docs, build, create GitHub release, and monitor
  the publishing workflow. Supports patch/minor/major auto-increment.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
disable-model-invocation: true
user-invocable: true
argument-hint: "<version|patch|minor|major>"
---

Please prepare and trigger a new release for version: $ARGUMENTS

Follow these steps:

**Output Format**: Use clear section headers, timestamps for each step, and progress indicators for release operations.

**Tool Validation**: First verify that required tools are installed (uv, gh, git) and the project has proper release configuration.

**Version Validation**: Validate that the provided version follows semantic versioning format (e.g., 1.2.3, 1.0.0-alpha.1).

1. **Run cleanup first**: Execute all cleanup steps (linting, formatting, type checking)
2. **Version increment options**: Support auto-increment with 'patch', 'minor', or 'major' keywords instead of explicit version numbers
3. **Update pyproject.toml**: Update the version field in pyproject.toml with the new version number
4. **Update version in docs**: Update all references to the next version based on the provided version number
5. **Check version**: Verify version in pyproject.toml and __init__.py match the target version and are greater than the current version
6. **Build test**: Run `uv build` to ensure the package builds correctly
7. **Create release**: Use `gh release create` with the appropriate version tag and release notes
8. **Monitor**: Track the publishing workflow with `gh run list` to ensure it completes successfully

**Error Handling**: If any step fails, attempt to fix the issue automatically. If unable to fix, report the specific error and suggested solutions before proceeding.

The version number provided is: $ARGUMENTS
