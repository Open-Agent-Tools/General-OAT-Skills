# Haystack Template

Deepset Haystack NLP pipelines.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── pipelines/
│   ├── __init__.py
│   ├── indexing_pipeline.py       # Document indexing pipeline
│   ├── rag_pipeline.py            # RAG query pipeline
│   └── search_pipeline.py         # Search and retrieval pipeline
├── components/
│   ├── __init__.py
│   ├── retrievers.py              # Custom retrieval components
│   ├── generators.py              # Custom generation components
│   └── preprocessors.py           # Document preprocessing components
├── data/
│   ├── documents/                 # Document storage
│   ├── __init__.py
│   └── document_store.py          # Document store configuration
├── agents/
│   ├── __init__.py
│   ├── search_agent.py            # Search agent with tools
│   └── qa_agent.py                # Question answering agent
├── tools/
│   ├── __init__.py
│   ├── web_tools.py               # Web scraping and search tools
│   └── file_tools.py              # File processing tools
├── main.py                        # Entry point - pipeline execution
├── config/
│   ├── __init__.py
│   └── haystack_config.py         # Haystack configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `haystack-ai`
- `sentence-transformers`
- `faiss-cpu` (or other vector databases)
- `python-dotenv`
