# AutoGen Template

Microsoft AutoGen conversational agents.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── agents/
│   ├── __init__.py
│   ├── user_proxy.py              # User proxy agent for human interaction
│   ├── assistant.py               # AI assistant agent
│   ├── code_executor.py           # Code execution agent
│   └── group_chat.py              # Group chat manager
├── functions/
│   ├── __init__.py
│   ├── code_functions.py          # Code execution functions
│   └── tool_functions.py          # Custom tool functions
├── workflows/
│   ├── __init__.py
│   ├── conversation_flow.py       # Conversation workflows
│   └── code_review_flow.py        # Code review workflows
├── config/
│   ├── __init__.py
│   ├── llm_config.py              # LLM configurations
│   └── agent_config.py            # Agent configurations
├── main.py                        # Entry point - conversation orchestration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `pyautogen`
- `openai` (or other LLM providers)
- `python-dotenv`
- `docker` (optional for code execution)
