# Google ADK Template

Google Agent Development Kit multi-agent system.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables (Google Gemini API keys)
├── agents/
│   ├── __init__.py
│   ├── agent_one/
│   │   ├── __init__.py
│   │   ├── agent.py               # Core agent logic, model, name, description
│   │   └── tools.py               # Optional: Custom Python functions for agent
│   └── agent_two/
│       ├── __init__.py
│       ├── agent.py               # Core agent logic, model, name, description
│       └── tools.py               # Optional: Custom Python functions for agent
├── main.py                        # Entry point - orchestrates multi-agent interactions
├── tests/                         # Optional: Unit and integration tests
│   ├── __init__.py
│   └── test_agents.py
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `google-adk` (Google Agent Development Kit)
- `python-dotenv` (for .env file support)
- Additional Google services as needed (e.g., `google-cloud-aiplatform`)
