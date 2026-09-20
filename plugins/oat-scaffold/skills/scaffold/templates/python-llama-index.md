# LlamaIndex Template

LlamaIndex RAG agents with document indexing.

## Directory Structure

```
{{PROJECT_NAME}}/
├── .env                           # Environment variables and API keys
├── agents/
│   ├── __init__.py
│   ├── query_agent.py             # Query engine agent
│   ├── chat_agent.py              # Chat engine agent
│   └── rag_agent.py               # RAG agent with custom tools
├── data/
│   ├── documents/                 # Document storage for indexing
│   ├── __init__.py
│   └── loaders.py                 # Document loaders and parsers
├── indices/
│   ├── __init__.py
│   ├── vector_index.py            # Vector store index setup
│   ├── graph_index.py             # Knowledge graph index
│   └── summary_index.py           # Summary index for documents
├── engines/
│   ├── __init__.py
│   ├── query_engine.py            # Custom query engines
│   ├── chat_engine.py             # Custom chat engines
│   └── retrieval_engine.py        # Custom retrieval engines
├── tools/
│   ├── __init__.py
│   ├── document_tools.py          # Document processing tools
│   └── search_tools.py            # Search and retrieval tools
├── main.py                        # Entry point - agent orchestration
├── config/
│   ├── __init__.py
│   └── llama_config.py            # LlamaIndex configuration
├── tests/
├── pyproject.toml
├── .gitignore
└── README.md
```

## Dependencies

- `llama-index`
- `llama-index-embeddings-openai`
- `llama-index-llms-openai`
- `llama-index-vector-stores-chroma` (or other vector stores)
- `python-dotenv`
