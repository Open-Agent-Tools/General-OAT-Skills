# Security Policy

## Supported Versions

All slash commands in this repository are maintained and supported:

| Version | Supported          | Notes |
| ------- | ------------------ | ----- |
| main    | :white_check_mark: | Current version |

## Security Considerations for Slash Commands

These slash commands are designed for Claude Code and include security considerations:

### Command Execution
- **User Context**: Commands execute with the user's permissions in Claude Code
- **File Operations**: Some commands modify files and git repositories
- **Code Execution**: Commands may run shell commands like git, pytest, ruff, etc.
- **Review Changes**: Always review changes before accepting them

### Sensitive Data
- **No Credentials**: Commands should never prompt for or store credentials
- **Environment Variables**: Commands may read environment variables like GITHUB_TOKEN
- **Git Operations**: Commands may create commits and interact with remote repositories
- **API Keys**: Be cautious when commands interact with external services

### Best Practices for Using Slash Commands
1. **Review Output**: Always review what commands plan to do before accepting
2. **Understand Commands**: Read command documentation before using
3. **Safe Defaults**: Commands should use safe, non-destructive defaults
4. **Version Control**: Use git to track all changes made by commands
5. **Test in Safe Environment**: Test new commands in test repositories first
6. **Keep Updated**: Pull latest command versions regularly

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please follow these steps:

### Where to Report
- **Email**: Send details to unseriousai@gmail.com with subject "SECURITY: claude_slash_commands_library"
- **GitHub**: For non-critical issues, you may create a private security advisory on GitHub

### What to Include
- Description of the vulnerability
- Steps to reproduce the issue
- Potential impact assessment
- Suggested fix (if any)
- Your contact information for follow-up

### Response Timeline
- **Initial Response**: Within 48 hours
- **Investigation**: 1-7 days depending on complexity
- **Fix Release**: Target within 14 days for critical issues
- **Public Disclosure**: After fix is released and users have time to update

### Process
1. **Report Received**: We acknowledge receipt and begin investigation
2. **Validation**: We reproduce and assess the vulnerability
3. **Fix Development**: We develop and test a fix
4. **Release**: We release a patched version
5. **Disclosure**: We publicly disclose details after users can update

### Security Updates
Security fixes are released immediately and are available via:
- GitHub repository updates (main branch)
- GitHub releases with security tags
- Security advisories on GitHub

Thank you for helping keep Claude Slash Commands Library secure!
