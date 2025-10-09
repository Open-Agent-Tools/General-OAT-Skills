# Contributing to Claude Slash Commands Library

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Making Changes](#making-changes)
- [Testing](#testing)
- [Submitting Changes](#submitting-changes)
- [Style Guide](#style-guide)
- [Reporting Issues](#reporting-issues)

## Code of Conduct

This project adheres to a [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/claude_slash_commands_library.git
   cd claude_slash_commands_library
   ```
3. **Add upstream remote**:
   ```bash
   git remote add upstream https://github.com/Open-Agent-Tools/claude_slash_commands_library.git
   ```

## Development Setup

### Prerequisites

- Text editor or IDE with markdown support
- Claude Code CLI (for testing slash commands)

### Creating/Editing Slash Commands

Each slash command is a markdown file that defines:
- Command description and usage
- Command behavior and workflow
- Examples and best practices

No build or compilation required - edit the `.md` files directly.

## Making Changes

1. **Create a new branch** for your changes:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes** following the [Style Guide](#style-guide)

3. **Test your slash command** in Claude Code:
   ```bash
   # Copy command to Claude Code commands directory
   cp your-command.md ~/.claude/commands/

   # Test the command in Claude Code CLI
   claude-code
   # Then use: /your-command
   ```

## Testing

### Testing Slash Commands

1. **Copy to Claude Code commands directory**:
   ```bash
   cp <command-name>.md ~/.claude/commands/
   ```

2. **Test in Claude Code CLI**:
   - Start a Claude Code session
   - Use `/your-command` to test functionality
   - Verify expected behavior
   - Test edge cases

3. **Verify command works**:
   - With different project types
   - In various directory structures
   - With expected inputs and flags

## Submitting Changes

### Before Submitting

- [ ] Slash command tested in Claude Code
- [ ] Markdown formatting is correct
- [ ] Documentation is clear and complete
- [ ] Examples are provided (if applicable)
- [ ] Commit messages are clear and descriptive

### Pull Request Process

1. **Push your changes** to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create a Pull Request** on GitHub

3. **Fill out the PR template** with:
   - Clear description of changes
   - Related issue numbers
   - Testing performed
   - Breaking changes (if any)

4. **Wait for review**:
   - Address reviewer feedback
   - Keep PR up to date with main branch
   - Be patient and respectful

### Commit Message Guidelines

```
type(scope): brief description

Longer description if needed, explaining:
- Why the change is necessary
- How it addresses the issue
- Any side effects or considerations

Fixes #123
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Test changes
- `chore`: Build, dependencies, tooling

## Style Guide

### Markdown Style

- **Line length**: Wrap at ~80-100 characters for readability
- **Headings**: Use ATX-style headings (`#`, `##`, etc.)
- **Code blocks**: Use fenced code blocks with language specification
- **Lists**: Use `-` for unordered lists, numbers for ordered lists

### Slash Command Structure

Each command file should include:

1. **Command name** as filename (e.g., `check.md`, `cleanup.md`)
2. **Description** of what the command does
3. **Usage examples** showing how to invoke it
4. **Implementation** detailing the workflow
5. **Error handling** and edge cases

Example structure:
```markdown
# /commandname - Brief Description

## Overview
What this command does and when to use it.

## Usage
/commandname [options]

## Implementation
Step-by-step workflow the command follows.

## Examples
Practical examples of usage.
```

### Documentation Best Practices

- Be clear and concise
- Include practical examples
- Document expected inputs and outputs
- Explain edge cases and limitations

### README Updates

- Update README when adding new commands
- Keep command list organized and up to date
- Update installation or usage instructions if needed

## Reporting Issues

### Bug Reports

Use the **Bug Report** template and include:
- Clear description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Environment details (OS, Python version)
- Error messages and stack traces

### Feature Requests

Use the **Feature Request** template and include:
- Clear description of the feature
- Use case and motivation
- Proposed API or interface
- Alternatives considered

### Security Issues

**Do not** open public issues for security vulnerabilities. Instead:
- Use [GitHub Security Advisories](https://github.com/Open-Agent-Tools/claude_slash_commands_library/security/advisories/new)
- Or email the maintainers directly
- See [SECURITY.md](SECURITY.md) for details

## Questions?

- Open a [Discussion](https://github.com/Open-Agent-Tools/claude_slash_commands_library/discussions) for questions
- Check existing issues and discussions first
- Be respectful and patient

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

Thank you for contributing to Claude Slash Commands Library! 🎉
