# Swarm Template

OpenAI Swarm lightweight agent framework.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── agents/
│   ├── __init__.py
│   ├── triage_agent.py            # Initial routing agent
│   ├── sales_agent.py             # Sales specialist agent
│   └── support_agent.py           # Support specialist agent
├── functions/
│   ├── __init__.py
│   ├── handoff_functions.py       # Agent handoff functions
│   └── tool_functions.py          # Custom tool functions
├── utils/
│   ├── __init__.py
│   ├── context_manager.py         # Context switching utilities
│   └── response_handler.py        # Response processing
├── main.py                        # Entry point - swarm orchestration
├── config/
│   ├── __init__.py
│   └── swarm_config.py            # Swarm configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `openai-swarm`
- `openai`
- `python-dotenv`
