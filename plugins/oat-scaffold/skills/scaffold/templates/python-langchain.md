# LangChain Agent Template

LangChain agent project with LangGraph support for multi-agent workflows.

## Directory Structure

```
{{PROJECT_NAME}}/
├── main.py or app.py              # Entry point - initializes agent, handles interactions
├── agents/
│   ├── __init__.py
│   ├── agent_name.py              # Agent definition with AgentExecutor, LLM, tools
│   ├── tools.py                   # Custom tools for external system interactions
│   └── prompts.py                 # Prompt templates separated from agent logic
├── config/
│   ├── __init__.py
│   └── settings.py                # API keys, model names, environment configs
├── data/                          # Optional data sources
│   ├── __init__.py
│   └── data_sources.py            # Vector store, knowledge base connections
├── utils/                         # Optional utilities
│   ├── __init__.py
│   └── helpers.py                 # Utility functions and helper classes
├── langgraph_app/                 # LangGraph orchestration (if using LangGraph)
│   ├── __init__.py
│   ├── graph.py                   # LangGraph state, nodes, edges definition
│   └── nodes.py                   # Individual node functions/classes
├── tests/
├── pyproject.toml
├── .env                           # Environment variables for API keys
├── .gitignore
└── README.md
```

## Dependencies

- `langchain`
- `langchain-community`
- `langgraph` (if multi-agent workflows needed)
- `python-dotenv` (for .env file support)
- Additional providers as needed (e.g., `langchain-openai`, `langchain-anthropic`)
