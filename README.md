# General OAT Skills

A curated collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), organized as a plugin using the [Agent Skills](https://agentskills.io) open standard.

## Install

### As a Plugin (Recommended)

From inside Claude Code, first add the marketplace:

```
/plugin marketplace add Open-Agent-Tools/General-OAT-Skills
```

Then install at the level you need. The form is `<plugin>@<marketplace>`, so the
marketplace name `general-oat-skills` is always on the **right** of the `@`:

**Everything** — all 11 skills:
```
/plugin install general-oat-skills@general-oat-skills
```
(The bundle plugin and the marketplace share a name. That one really is doubled.)

**Bundles** — thematic groups:
```
/plugin install oat-core@general-oat-skills      # check, cleanup, test, load, publish-python, publish-rust
/plugin install oat-quality@general-oat-skills   # qa, scope-it, teach-me
/plugin install oat-agents@general-oat-skills    # scaffold, run-adk-evals
```

**Individual skills** — pick exactly what you need:
```
/plugin install oat-check@general-oat-skills
/plugin install oat-cleanup@general-oat-skills
/plugin install oat-test@general-oat-skills
/plugin install oat-load@general-oat-skills
/plugin install oat-publish-python@general-oat-skills
/plugin install oat-publish-rust@general-oat-skills
/plugin install oat-qa@general-oat-skills
/plugin install oat-scope-it@general-oat-skills
/plugin install oat-scaffold@general-oat-skills
/plugin install oat-teach-me@general-oat-skills
/plugin install oat-run-adk-evals@general-oat-skills
```

Then `/reload-plugins` to pick them up without restarting.

### Manual

Clone and symlink into your project:

```bash
git clone https://github.com/Open-Agent-Tools/General-OAT-Skills.git
ln -s "$(pwd)/General-OAT-Skills/skills" /path/to/your-project/.claude/skills
```

Or copy individual skill directories into `.claude/skills/` (project-level) or `~/.claude/skills/` (global).

## Skills

Plugin skills are namespaced by the plugin you installed them from, as
`/<plugin>:<skill>`. The same skill therefore has a different name depending on
which target you chose:

| installed via | invoke `check` as |
|---|---|
| `general-oat-skills` | `/general-oat-skills:check` |
| `oat-core` | `/oat-core:check` |
| `oat-check` | `/oat-check:check` |

Only the manual install above gives you a bare `/check`.

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

```
General-OAT-Skills/
├── .claude-plugin/
│   ├── marketplace.json      # all 15 install targets (full, bundles, individual)
│   └── plugin.json           # the root is also the `general-oat-skills` plugin
├── skills/                   # canonical skill definitions
│   ├── check/SKILL.md
│   ├── cleanup/SKILL.md
│   ├── load/SKILL.md
│   ├── publish-python/SKILL.md
│   ├── publish-rust/SKILL.md
│   ├── qa/SKILL.md
│   ├── run-adk-evals/SKILL.md
│   ├── scope-it/SKILL.md
│   ├── teach-me/SKILL.md
│   ├── test/SKILL.md
│   └── scaffold/
│       ├── SKILL.md
│       └── templates/        # 18 project templates
├── skills-core/              # bundle: check, cleanup, test, load, publish-python, publish-rust
│   ├── .claude-plugin/plugin.json
│   └── skills/               # symlinks → ../../skills/<name>
├── skills-quality/           # bundle: qa, scope-it, teach-me
├── skills-agents/            # bundle: scaffold, run-adk-evals
├── skill-*/                  # individual: one skill each (skill-check, skill-qa, ...)
│   ├── .claude-plugin/plugin.json
│   └── skills/<name>         # symlink → ../../skills/<name>
├── .github/workflows/
│   └── validate-plugins.yml  # CI: validates every marketplace install target
├── CODE_OF_CONDUCT.txt
├── CONTRIBUTING.txt
├── LICENSE
└── SECURITY.txt
```

## Contributing

See [CONTRIBUTING.txt](CONTRIBUTING.txt) for full guidelines.

To add a new skill:

1. Create `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name`, `description`, `allowed-tools`)
2. Add the wrapper roots so the skill ships through every install target:
   ```bash
   mkdir -p skill-<name>/skills && ln -s ../../skills/<name> skill-<name>/skills/<name>
   ln -s ../../skills/<name> skills-<bundle>/skills/<name>
   ```
   The `skills/` level is required — plugin skills are discovered at
   `<plugin-root>/skills/<name>/SKILL.md`, and a symlink one level shallower installs
   cleanly while shipping zero skills.
3. Give the new wrapper root its own `skill-<name>/.claude-plugin/plugin.json`. Every
   directory a marketplace `source` points at is a plugin root and needs a manifest —
   without one the entry still lists in `/plugin`, but nothing installs.
4. Add an `oat-<name>` entry to `.claude-plugin/marketplace.json` pointing at `./skill-<name>`,
   and bump the version in the marketplace and in every `plugin.json`
5. Verify with `claude plugin validate .` **and** `claude plugin validate skill-<name>`, then
   install the target and confirm `claude plugin details oat-<name>@general-oat-skills`
   reports `Skills (1)`
6. Submit a pull request

## License

Apache License 2.0 — see [LICENSE](LICENSE).
