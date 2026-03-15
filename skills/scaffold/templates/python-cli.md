# Python CLI Template

Python CLI application with Click/Typer, argument parsing, and structured commands.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   └── {{PROJECT_NAME}}/
│       ├── __init__.py            # Package init with version
│       ├── cli.py                 # Main CLI entry point with Typer app
│       ├── commands/
│       │   ├── __init__.py
│       │   └── default.py         # Default command group
│       ├── core.py                # Core business logic
│       └── utils.py               # Utility functions
├── tests/
│   ├── __init__.py
│   ├── conftest.py
│   └── test_cli.py               # CLI integration tests
├── pyproject.toml                # With [project.scripts] entry point
├── .github/
│   └── workflows/
│       └── ci.yml
├── .gitignore
├── .editorconfig
├── README.md
└── LICENSE
```

## Dependencies

- `typer` (with `rich` for pretty output)
- `rich`
- `python-dotenv`
- `pytest` (dev)
- `pytest-cov` (dev)
- `ruff` (dev)
- `mypy` (dev)
