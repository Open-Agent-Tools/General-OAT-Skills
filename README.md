# General OAT Skills

A curated collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), organized as a plugin using the [Agent Skills](https://agentskills.io) open standard.

## Install

### As a Plugin (Recommended)

From inside Claude Code, first add the marketplace:

```
/plugin marketplace add Open-Agent-Tools/General-OAT-Skills
```

Then install at the level you need:

**Everything** — all 9 skills:
```
/plugin install general-oat-skills@general-oat-skills
```

**Bundles** — thematic groups:
```
/plugin install general-oat-skills@oat-core       # check, cleanup, test, load, publish
/plugin install general-oat-skills@oat-quality     # qa, teach-me
/plugin install general-oat-skills@oat-agents      # scaffold, run-adk-evals
```

**Individual skills** — pick exactly what you need:
```
/plugin install general-oat-skills@oat-check
/plugin install general-oat-skills@oat-cleanup
/plugin install general-oat-skills@oat-test
/plugin install general-oat-skills@oat-load
/plugin install general-oat-skills@oat-publish
/plugin install general-oat-skills@oat-qa
/plugin install general-oat-skills@oat-scaffold
/plugin install general-oat-skills@oat-teach-me
/plugin install general-oat-skills@oat-run-adk-evals
```

### Manual

Clone and symlink into your project:

```bash
git clone https://github.com/Open-Agent-Tools/General-OAT-Skills.git
ln -s "$(pwd)/General-OAT-Skills/skills" /path/to/your-project/.claude/skills
```

Or copy individual skill directories into `.claude/skills/` (project-level) or `~/.claude/skills/` (global).

## Skills

### Core Dev Workflow

| Skill | Description |
|-------|-------------|
| `/check` | Quick project health check — git status, recent commits, GitHub Actions, package build |
| `/cleanup` | Full code quality pipeline — ruff, mypy, pytest — with validation loops |
| `/test` | Run pytest suite with coverage reporting and auto-fix on failures |
| `/load [directory]` | Load all markdown files from a directory into context |
| `/publish <version>` | Release management — cleanup, version bump, build, GitHub release |

### Quality & Learning

| Skill | Description |
|-------|-------------|
| `/qa [focus]` | Multi-agent QA audit — code quality, architecture, security, tests, CI/CD, docs |
| `/teach-me <topic> [flags]` | Interactive lesson-based teaching with web research, quizzes, and difficulty levels |

### AI Agent Development

| Skill | Description |
|-------|-------------|
| `/scaffold <template> [name]` | Create a new project from 18 templates with full setup |
| `/run-adk-evals <folder>` | Run Google ADK evaluations with detailed results and rate limiting |

### Scaffold Templates

**General Purpose**
`python-lib` `python-cli` `python-web` `python-data` `node-lib` `node-web` `rust-lib` `go-cli`

**AI Agent Frameworks**
`python-langchain` `python-adk` `python-crewai` `python-autogen` `python-swarm` `python-phidata` `python-llama-index` `python-haystack` `python-semantic-kernel` `python-agency-swarm`

## Skill Behaviors

- **Side-effect protection** — `cleanup`, `publish`, `scaffold`, and `qa` have `disable-model-invocation: true`, so Claude won't auto-trigger them. They must be explicitly invoked.
- **Context isolation** — `load` and `check` run in forked context (`context: fork`) to avoid polluting your main conversation.
- **Argument hints** — Skills that accept parameters show usage hints in autocomplete.

## Project Structure

```
General-OAT-Skills/
├── .claude-plugin/
│   ├── marketplace.json      # all install targets (full, bundles, individual)
│   └── plugin.json
├── skills/                   # canonical skill definitions
│   ├── check/SKILL.md
│   ├── cleanup/SKILL.md
│   ├── load/SKILL.md
│   ├── publish/SKILL.md
│   ├── qa/SKILL.md
│   ├── run-adk-evals/SKILL.md
│   ├── teach-me/SKILL.md
│   ├── test/SKILL.md
│   └── scaffold/
│       ├── SKILL.md
│       └── templates/        # 18 project templates
├── skills-core/              # bundle: symlinks → check, cleanup, test, load, publish
├── skills-quality/           # bundle: symlinks → qa, teach-me
├── skills-agents/            # bundle: symlinks → scaffold, run-adk-evals
├── CODE_OF_CONDUCT.txt
├── CONTRIBUTING.txt
├── LICENSE
└── SECURITY.txt
```

## Contributing

See [CONTRIBUTING.txt](CONTRIBUTING.txt) for full guidelines.

To add a new skill:

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name`, `description`, `allowed-tools`)
2. Test locally by symlinking the repo's `skills/` directory into a project
3. Submit a pull request

## License

Apache License 2.0 — see [LICENSE](LICENSE).
