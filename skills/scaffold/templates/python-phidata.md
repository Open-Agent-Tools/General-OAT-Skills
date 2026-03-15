# Phidata Template

Phidata AI assistant framework.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── assistants/
│   ├── __init__.py
│   ├── research_assistant.py      # Research assistant with tools
│   ├── data_assistant.py          # Data analysis assistant
│   └── web_assistant.py           # Web search assistant
├── knowledge/
│   ├── __init__.py
│   ├── documents/                 # Document storage
│   ├── vector_db.py               # Vector database setup
│   └── knowledge_base.py          # Knowledge base management
├── tools/
│   ├── __init__.py
│   ├── web_tools.py               # Web search and scraping tools
│   ├── data_tools.py              # Data processing tools
│   └── file_tools.py              # File manipulation tools
├── workflows/
│   ├── __init__.py
│   └── research_workflow.py       # Research workflow orchestration
├── main.py                        # Entry point - assistant interactions
├── config/
│   ├── __init__.py
│   └── phi_config.py              # Phidata configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `phidata`
- `openai` (or other LLM providers)
- `pgvector` (for PostgreSQL vector storage)
- `python-dotenv`
