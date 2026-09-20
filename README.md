# General OAT Skills

A curated collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), organized as a plugin using the [Agent Skills](https://agentskills.io) open standard.

## Install

From inside Claude Code, add the marketplace once:

```
/plugin marketplace add Open-Agent-Tools/General-OAT-Skills
```

Then install the skills you want. Each skill is its own plugin, so you take
only what you need:

```
/plugin install oat-check@general-oat-skills
/plugin install oat-cleanup@general-oat-skills
/plugin install oat-load@general-oat-skills
/plugin install oat-publish-python@general-oat-skills
/plugin install oat-publish-rust@general-oat-skills
/plugin install oat-qa@general-oat-skills
/plugin install oat-run-adk-evals@general-oat-skills
/plugin install oat-scaffold@general-oat-skills
/plugin install oat-scope-it@general-oat-skills
/plugin install oat-teach-me@general-oat-skills
/plugin install oat-test@general-oat-skills
```

The form is `<plugin>@<marketplace>` — the marketplace name is always on the
right of the `@`. Run `/reload-plugins` afterwards to pick them up without
restarting.

### Manual

Copy a skill directory into `.claude/skills/` (project) or `~/.claude/skills/`
(global):

```bash
git clone https://github.com/Open-Agent-Tools/General-OAT-Skills.git
cp -R General-OAT-Skills/plugins/oat-check/skills/check ~/.claude/skills/
```

## Skills

Plugin skills are namespaced by the plugin that provides them, so `check`
installed from `oat-check` is invoked as `/oat-check:check`. A bare `/check`
also works unless another command already uses that name. The manual install
above gives you a bare `/check` only.

### Core Dev Workflow

| Skill | Description |
|-------|-------------|
| `check` | Quick project health check — git status, recent commits, GitHub Actions, package build |
| `cleanup` | Full code quality pipeline — ruff, mypy, pytest — with validation loops |
| `test` | Run pytest suite with coverage reporting and auto-fix on failures |
| `load [directory]` | Load all markdown files from a directory into context |
| `publish-python <version>` | Python release — cleanup, version bump, uv build, GitHub release |
| `publish-rust <bump>` | Rust crate/workspace release — verify gates, version bump, signed tag, CI or direct crates.io publish |

### Quality & Learning

| Skill | Description |
|-------|-------------|
| `qa [focus]` | Multi-agent QA audit — code quality, architecture, security, tests, CI/CD, docs |
| `scope-it [task]` | One-question-at-a-time interview that turns a vague task into a ready-to-use prompt — what, why, constraints, done |
| `teach-me <topic> [flags]` | Interactive lesson-based teaching with web research, quizzes, and difficulty levels |

### AI Agent Development

| Skill | Description |
|-------|-------------|
| `scaffold <template> [name]` | Create a new project from 18 templates with full setup |
| `run-adk-evals <folder>` | Run Google ADK evaluations with detailed results and rate limiting |

### Scaffold Templates

**General Purpose**
`python-lib` `python-cli` `python-web` `python-data` `node-lib` `node-web` `rust-lib` `go-cli`

**AI Agent Frameworks**
`python-langchain` `python-adk` `python-crewai` `python-autogen` `python-swarm` `python-phidata` `python-llama-index` `python-haystack` `python-semantic-kernel` `python-agency-swarm`

## Skill Behaviors

- **Side-effect protection** — `cleanup`, `publish-python`, `publish-rust`, `scaffold`, and `qa` have `disable-model-invocation: true`, so Claude won't auto-trigger them. They must be explicitly invoked.
- **Context isolation** — `load` and `check` run in forked context (`context: fork`) to avoid polluting your main conversation.
- **Argument hints** — Skills that accept parameters show usage hints in autocomplete.

## Project Structure

Each skill is a self-contained plugin. There is no shared directory and no
symlinks, so a checkout behaves the same on macOS, Linux, and Windows.

```
General-OAT-Skills/
├── .claude-plugin/
│   └── marketplace.json          # the 11 install targets
└── plugins/
    ├── oat-check/
    │   ├── .claude-plugin/plugin.json
    │   └── skills/check/SKILL.md
    ├── oat-scaffold/
    │   ├── .claude-plugin/plugin.json
    │   └── skills/scaffold/
    │       ├── SKILL.md
    │       └── templates/        # 18 project templates
    └── ...                       # one directory per skill
```

## Contributing

See [CONTRIBUTING.txt](CONTRIBUTING.txt) for full guidelines.

To add a new skill:

1. Create `plugins/oat-<name>/skills/<name>/SKILL.md` with YAML frontmatter
   (`name`, `description`, `allowed-tools`)
2. Create `plugins/oat-<name>/.claude-plugin/plugin.json` with `name` set to
   `oat-<name>` — every directory a marketplace `source` points at is a plugin
   root and needs a manifest
3. Add an `oat-<name>` entry to `.claude-plugin/marketplace.json` pointing at
   `./plugins/oat-<name>`, and bump the version in both files
4. Add the install command to the README — CI checks every plugin has one
5. Verify with `claude plugin validate .` and
   `claude plugin validate plugins/oat-<name> --strict`
6. Submit a pull request

Never commit a symlink. Git checks them out as plain text files on Windows,
which turns a skill directory into an unusable stub; CI rejects them.

## License

Apache License 2.0 — see [LICENSE](LICENSE).
