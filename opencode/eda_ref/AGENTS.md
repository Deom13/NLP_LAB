# AGENTS.md

Guidelines for AI agents (and human contributors) working on this project.

## Project Overview

This project performs an Exploratory Data Analysis (EDA) on German doctor reviews using Python. The primary output is a full analysis report with statistical summaries of textual corpora using visualizations (plots, graphs).

---

## Environment Setup

**Always use a virtual environment.** Never install packages into the system Python.

### Initial Setup

```bash
python3 -m venv .venv
source .venv/bin/activate       # Linux/macOS
# .venv\Scripts\activate        # Windows
pip install --upgrade pip
pip install -r requirements.txt
```

### Running Code

- Activate the virtual environment before running any Python script or Jupyter notebook.
- Run scripts with: `python src/<script>.py`

### Adding Dependencies

1. Install the package: `pip install <package>`
2. Update the pinned file: `pip freeze > requirements.txt`

Do **not** manually edit `requirements.txt` version pins — always regenerate it via `pip freeze`.

---

## Project Structure

```
.
├── AGENTS.md
├── requirements.txt
├── .venv/                  # Virtual environment (not committed)
├── data/
│   ├── raw/                # Original, unmodified source data
│   └── processed/          # Cleaned/transformed data
├── src/                    # Reusable Python modules
│   ├── __init__.py
│   ├── data_loader.py      # Text cleaning and normalization
│   ├── preprocess.py       # Text cleaning and normalization
│   └── utils.py            # Shared helpers
├── outputs/
│   ├── figures/            # Saved plots and visualizations
│   └── reports/            # Exported summaries
```

---

## Python Standards

### Version

Use **Python 3.10+**. Specify the exact version in a `.python-version` file if using `pyenv`.

### Style

- Follow [PEP 8](https://peps.python.org/pep-0008/).
- Use 4 spaces for indentation
- Use meaningful variable names. Functions and variables use `snake_case`. Classes use `CamelCase`. Constants are `UPPER_SNAKE_CASE`.
- If installed, format code with **black** (line length: 88, the black default).
- If installed, lint with **ruff**.
- Run both before committing:

```bash
black src/ 
ruff check src/ 
```

### Type Hints

Use type hints for all function signatures in `src/`:

```python
def clean_text(text: str, lowercase: bool = True) -> str:
    ...
```

### Imports

Order imports as: standard library → third-party → local. Use `ruff` to enforce this automatically.

---

## Security and Best Practices

- **Security**: Never hard-code secrets (API keys, tokens); use the provided configuration system. 
- **Error Handling**: Handle exceptions gracefully – do not expose internal stack traces to users. Log errors using the Python logging module.

---

## Data Loading

1. **Create data loading script** (`src/data_loader.py`)
Following code describes how to download the data into a local file, read it into a dataframe, and get a first impression of the data.
```python
from fhnw.nlp.utils.storage import download, load_dataframe

# download the file into a local file
data_file = "data/raw/german_doctor_reviews_tokenized.parq"
download('https://drive.switch.ch/index.php/s/0hE8wO4FbfGIJld/download', data_file)

# read the file into a dataframe
data = load_dataframe(data_file)

# get an impression of the data
print(data.head())
```
   
---

## Text EDA Guidelines

When performing EDA on text data, ask the user what kind of analysis you should perform.

In case the user asks for guidance, come up with your own suggestions or use one of the following analyses as a baseline:

1. **Data loading and inspection** — shape, dtypes, null counts, sample rows
2. **Basic text statistics** — document count, token count per document (mean/median/max), vocabulary size
3. **Text cleaning** — lowercasing, punctuation removal, stopword removal, whitespace normalization
4. **Token frequency analysis** — top-N unigrams, bigrams, trigrams; frequency distribution plots
5. **Visualizations** — word clouds, bar charts of top terms, document length histograms
6. **Lexical diversity** — type-token ratio (TTR)
7. **Language detection** (if corpus may be multilingual)

All figures must be saved to `outputs/figures/` in addition to being displayed inline.

---

## .gitignore Essentials

The following must never be committed:

```
.venv/
__pycache__/
*.pyc
.ipynb_checkpoints/
data/raw/          # Raw data may be sensitive or large
outputs/
.env
```

---

## Common Commands Reference

| Task | Command |
|---|---|
| Activate venv | `source .venv/bin/activate` |
| Install deps | `pip install -r requirements.txt` |
| Update deps | `pip freeze > requirements.txt` |
| Format code | `black src/` |
| Lint code | `ruff check src/` |
