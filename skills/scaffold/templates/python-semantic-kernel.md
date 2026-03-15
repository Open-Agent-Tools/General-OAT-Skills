# Semantic Kernel Template

Microsoft Semantic Kernel plugin architecture.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── plugins/
│   ├── __init__.py
│   ├── core_plugin/
│   │   ├── __init__.py
│   │   ├── text_functions.py      # Text processing functions
│   │   └── math_functions.py      # Mathematical functions
│   └── web_plugin/
│       ├── __init__.py
│       └── search_functions.py    # Web search functions
├── skills/
│   ├── __init__.py
│   ├── conversation_skill.py      # Conversation management
│   └── planning_skill.py          # Planning and orchestration
├── planners/
│   ├── __init__.py
│   ├── sequential_planner.py      # Sequential plan execution
│   └── action_planner.py          # Action-based planning
├── memory/
│   ├── __init__.py
│   ├── semantic_memory.py         # Semantic memory store
│   └── volatile_memory.py         # Temporary memory
├── agents/
│   ├── __init__.py
│   └── sk_agent.py                # Semantic Kernel agent
├── main.py                        # Entry point - kernel orchestration
├── config/
│   ├── __init__.py
│   └── sk_config.py               # Semantic Kernel configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `semantic-kernel`
- `openai` (or other LLM providers)
- `python-dotenv`
- `aiohttp` (for async operations)
