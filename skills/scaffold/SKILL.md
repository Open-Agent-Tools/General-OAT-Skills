---
name: scaffold
description: >-
  Creates a new project from templates with full setup including directory
  structure, dependency management, git initialization, CI/CD, testing,
  and documentation. Supports Python, Node.js, Rust, Go, and 14 AI agent
  framework templates. Use when the user wants to start a new project,
  bootstrap a codebase, or create a project from a template.
allowed-tools:
  - Bash
  - Read
  - Edit
  - Grep
  - Glob
disable-model-invocation: true
user-invocable: true
argument-hint: "<template-name> [project-name]"
---

Create a new project using the specified template: $ARGUMENTS

**Output Format**: Use clear section headers, timestamps for each step, and progress indicators for setup operations.

**Tool Validation**: First verify that required tools are installed (git, uv, gh) and the target directory is valid.

**Python Project Defaults**: All Python projects use UV for virtual environment management and package installation. Never use pip, pipenv, or conda.

**Template Options**:
- `python-lib` - Python library with pyproject.toml, tests, CI/CD
- `python-cli` - Python CLI application with Click/Typer
- `python-web` - FastAPI web application
- `python-data` - Data science project with Jupyter notebooks
- `python-langchain` - LangChain agent project with LangGraph support
- `python-adk` - Google Agent Development Kit multi-agent system
- `python-crewai` - CrewAI multi-agent orchestration framework
- `python-autogen` - Microsoft AutoGen conversational agents
- `python-swarm` - OpenAI Swarm lightweight agent framework
- `python-phidata` - Phidata AI assistant framework
- `python-llama-index` - LlamaIndex RAG agents with document indexing
- `python-haystack` - Deepset Haystack NLP pipelines
- `python-semantic-kernel` - Microsoft Semantic Kernel plugin architecture
- `python-agency-swarm` - Agency Swarm hierarchical agent framework
- `node-lib` - Node.js library with TypeScript
- `node-web` - React/Next.js web application
- `rust-lib` - Rust library with Cargo.toml
- `go-cli` - Go CLI application
- `custom` - Interactive template selection

**Template Loading**: Read the template definition from `${CLAUDE_SKILL_DIR}/templates/<template-name>.md` for the selected template's directory structure and dependencies.

**Project Setup Steps**:

1. **Template Selection**: Determine template from argument or prompt interactively
2. **Load Template**: Read the template file from `${CLAUDE_SKILL_DIR}/templates/<template-name>.md`
3. **Directory Creation**: Create project directory with proper naming conventions
4. **Template Scaffolding**: Create the directory structure and files defined in the template
5. **Dependency Management**:
   - For Python: Use `uv init` and `uv add` exclusively for all dependencies
   - For Python: Create virtual environment with `uv venv` and activate with `uv run`
   - For other languages: Install language-specific dependencies
6. **Git Initialization**: Initialize repo, create .gitignore, initial commit
7. **CI/CD Setup**: Create GitHub Actions workflows, testing and linting pipelines
8. **Development Environment**: Setup IDE config files, development scripts
9. **Documentation**: Generate README, CONTRIBUTING.md, documentation framework
10. **Testing Framework**: Setup test directory, sample tests, configure runners
11. **Quality Tools**: Configure linting, formatters, type checking
12. **Final Validation**:
    - For Python: Run `uv run python -m pytest` and `uv build`
    - For other languages: Run initial build/compile and basic tests

**Interactive Prompts**:
- Project name (default: directory name)
- Author name and email
- License type (MIT, Apache, etc.)
- Description
- Python version (for Python projects)

**Template Variables**:
- `{{PROJECT_NAME}}` - Project name
- `{{AUTHOR_NAME}}` - Author name
- `{{AUTHOR_EMAIL}}` - Author email
- `{{DESCRIPTION}}` - Project description
- `{{LICENSE}}` - License type
- `{{YEAR}}` - Current year
- `{{PYTHON_VERSION}}` - Python version

**Error Handling**: If any step fails, attempt to fix the issue automatically. If unable to fix, report the specific error and provide cleanup instructions.

**Post-Setup Instructions**:
- Display next steps for development
- Show available commands (test, build, etc.)
- Provide quick start guide
- Suggest development workflow
