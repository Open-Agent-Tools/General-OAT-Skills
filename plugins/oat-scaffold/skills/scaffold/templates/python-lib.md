# Python Library Template

Standard Python library with src layout, modern packaging, and full CI/CD.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   └── {{PROJECT_NAME}}/
│       ├── __init__.py            # Package init with version
│       ├── core.py                # Core library functionality
│       └── utils.py               # Utility functions
├── tests/
│   ├── __init__.py
│   ├── conftest.py                # Shared test fixtures
│   └── test_core.py              # Core module tests
├── docs/
│   └── index.md                  # Documentation entry point
├── pyproject.toml                # Project metadata and build config
├── .github/
│   └── workflows/
│       └── ci.yml                # CI pipeline: lint, test, publish
├── .gitignore
├── .editorconfig
├── README.md
├── LICENSE
└── CHANGELOG.md
```

## Dependencies

- `pytest` (dev)
- `pytest-cov` (dev)
- `ruff` (dev)
- `mypy` (dev)
