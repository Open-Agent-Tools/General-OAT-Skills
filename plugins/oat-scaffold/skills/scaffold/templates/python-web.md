# Python Web Template

FastAPI web application with async support, structured routes, and Docker.

## Directory Structure

```
{{PROJECT_NAME}}/
├── src/
│   └── {{PROJECT_NAME}}/
│       ├── __init__.py
│       ├── main.py                # FastAPI app initialization
│       ├── api/
│       │   ├── __init__.py
│       │   ├── routes/
│       │   │   ├── __init__.py
│       │   │   └── health.py      # Health check endpoint
│       │   ├── deps.py            # Dependency injection
│       │   └── middleware.py      # Custom middleware
│       ├── models/
│       │   ├── __init__.py
│       │   └── schemas.py         # Pydantic models
│       ├── services/
│       │   ├── __init__.py
│       │   └── base.py            # Service layer
│       └── config.py              # Settings via pydantic-settings
├── tests/
│   ├── __init__.py
│   ├── conftest.py                # Test client fixture
│   └── test_health.py
├── Dockerfile
├── docker-compose.yml
├── pyproject.toml
├── .github/
│   └── workflows/
│       └── ci.yml
├── .env.example
├── .gitignore
├── .editorconfig
├── README.md
└── LICENSE
```

## Dependencies

- `fastapi`
- `uvicorn[standard]`
- `pydantic-settings`
- `python-dotenv`
- `httpx` (dev, for async test client)
- `pytest` (dev)
- `pytest-asyncio` (dev)
- `pytest-cov` (dev)
- `ruff` (dev)
- `mypy` (dev)
