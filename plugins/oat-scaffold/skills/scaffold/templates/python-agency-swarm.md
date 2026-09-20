# Agency Swarm Template

Agency Swarm hierarchical agent framework.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── agency/
│   ├── __init__.py
│   ├── ceo/
│   │   ├── __init__.py
│   │   ├── ceo.py                 # CEO agent - main orchestrator
│   │   └── instructions.md        # CEO instructions and goals
│   ├── developer/
│   │   ├── __init__.py
│   │   ├── developer.py           # Developer agent
│   │   ├── instructions.md        # Developer instructions
│   │   └── tools/                 # Developer-specific tools
│   │       ├── __init__.py
│   │       └── code_tools.py
│   └── researcher/
│       ├── __init__.py
│       ├── researcher.py          # Researcher agent
│       ├── instructions.md        # Researcher instructions
│       └── tools/                 # Research-specific tools
│           ├── __init__.py
│           └── search_tools.py
├── shared_tools/
│   ├── __init__.py
│   ├── file_tools.py              # Shared file manipulation tools
│   └── communication_tools.py     # Inter-agent communication
├── workflows/
│   ├── __init__.py
│   └── project_workflow.py        # Project execution workflow
├── main.py                        # Entry point - agency orchestration
├── config/
│   ├── __init__.py
│   └── agency_config.py           # Agency configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `agency-swarm`
- `openai`
- `python-dotenv`
- Additional tools as needed (e.g., `requests`, `beautifulsoup4`)
