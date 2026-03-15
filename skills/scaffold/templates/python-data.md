# Python Data Science Template

Data science project with Jupyter notebooks, analysis pipelines, and visualization.

## Directory Structure

```
{{PROJECT_NAME}}/
├── notebooks/
│   └── 01_exploration.ipynb       # Initial data exploration notebook
├── src/
│   └── {{PROJECT_NAME}}/
│       ├── __init__.py
│       ├── data/
│       │   ├── __init__.py
│       │   └── loader.py          # Data loading utilities
│       ├── features/
│       │   ├── __init__.py
│       │   └── engineering.py     # Feature engineering
│       ├── models/
│       │   ├── __init__.py
│       │   └── train.py           # Model training
│       └── visualization/
│           ├── __init__.py
│           └── plots.py           # Visualization helpers
├── data/
│   ├── raw/                       # Raw immutable data
│   ├── processed/                 # Cleaned and transformed data
│   └── .gitkeep
├── outputs/
│   ├── figures/                   # Generated plots
│   └── models/                    # Trained model artifacts
├── tests/
│   ├── __init__.py
│   └── test_data.py
├── pyproject.toml
├── .gitignore                     # Includes data/ and outputs/ patterns
├── .editorconfig
├── README.md
└── LICENSE
```

## Dependencies

- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scikit-learn`
- `jupyter`
- `ipykernel`
- `python-dotenv`
- `pytest` (dev)
- `ruff` (dev)
