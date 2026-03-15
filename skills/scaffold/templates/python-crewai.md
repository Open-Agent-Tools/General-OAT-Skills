# CrewAI Template

CrewAI multi-agent orchestration framework.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── agents/
│   ├── __init__.py
│   ├── researcher.py              # Research agent with role, goal, backstory
│   ├── writer.py                  # Writer agent with specialized skills
│   └── reviewer.py                # Review agent for quality control
├── tasks/
│   ├── __init__.py
│   ├── research_tasks.py          # Research task definitions
│   ├── writing_tasks.py           # Writing task definitions
│   └── review_tasks.py            # Review task definitions
├── tools/
│   ├── __init__.py
│   ├── search_tools.py            # Custom search and research tools
│   └── file_tools.py              # File manipulation tools
├── crews/
│   ├── __init__.py
│   └── main_crew.py               # Crew orchestration and workflow
├── main.py                        # Entry point - crew execution
├── config/
│   ├── __init__.py
│   └── agents.yaml                # Agent configurations
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `crewai`
- `crewai[tools]` (for additional tools)
- `python-dotenv`
- Additional tools as needed (e.g., `duckduckgo-search`, `requests`)
