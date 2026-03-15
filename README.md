# General OAT Skills

A curated collection of powerful skills for Claude Code, built on the modern [Agent Skills](https://agentskills.io) open standard. Enhance your development workflow with project scaffolding, code quality, testing, review, and release management.

## Installation

### Plugin Install (Recommended)

```bash
claude plugin install github:Open-Agent-Tools/General-OAT-Skills
```

### Manual Install

Clone the repository and symlink the skills directory into your project:

```bash
git clone https://github.com/Open-Agent-Tools/General-OAT-Skills.git
ln -s /path/to/General-OAT-Skills/skills .claude/skills
```

Or copy individual skills into `~/.claude/skills/` for personal (all-project) scope.

## Available Skills

### Core Dev Workflow

| Skill | Command | Description |
|-------|---------|-------------|
| **check** | `/check` | Quick project health check: git status, recent commits, GitHub Actions, package build |
| **cleanup** | `/cleanup` | Full code quality pipeline: ruff, mypy, pytest, then commit and push fixes |
| **load** | `/load [directory]` | Load all markdown files from a directory into context for analysis |
| **test** | `/test` | Comprehensive pytest suite with coverage reporting and auto-fix |
| **review** | `/review [file-or-dir]` | Code review for best practices, performance, and error handling |
| **publish** | `/publish <version>` | Release management: cleanup, version bump, build, GitHub release |

### AI Agent Development

| Skill | Command | Description |
|-------|---------|-------------|
| **scaffold** | `/scaffold <template> [name]` | Create new projects from 18 templates (Python, Node.js, Rust, Go, AI frameworks) |
| **run-adk-evals** | `/run-adk-evals <folder>` | Run Google ADK evaluations with detailed results and rate limiting |

### Scaffold Templates

**Python**: `python-lib`, `python-cli`, `python-web`, `python-data`

**AI Agent Frameworks**: `python-langchain`, `python-adk`, `python-crewai`, `python-autogen`, `python-swarm`, `python-phidata`, `python-llama-index`, `python-haystack`, `python-semantic-kernel`, `python-agency-swarm`

**Other Languages**: `node-lib`, `node-web`, `rust-lib`, `go-cli`

## Skill Behavior

Skills use the modern Claude Code skills format with these behaviors:

- **Auto-trigger disabled** on `cleanup`, `publish`, `scaffold` — these have side effects and must be explicitly invoked
- **Fork context** on `load`, `review` — these run in isolated subagent context to protect main conversation
- **Argument hints** shown in autocomplete for skills that accept parameters

## Contributing

See [CONTRIBUTING.txt](CONTRIBUTING.txt) for guidelines on adding new skills.

### Quick Start

1. Create a new skill directory: `skills/<skill-name>/SKILL.md`
2. Add YAML frontmatter with `name`, `description`, and `allowed-tools`
3. Test locally: `ln -s /path/to/repo/skills ~/.claude/skills`
4. Submit a pull request

## License

This project is licensed under the Apache License 2.0 — see the [LICENSE](LICENSE) file for details.
