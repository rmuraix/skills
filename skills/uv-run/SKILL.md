---
name: uv-run
description: Use `uv run` instead of `python` or `python3` for all Python execution. Trigger whenever the user wants to run a Python script, module, or tests, or needs temporary packages via `uv run --with`.
license: MIT
---

# uv run: How to Execute Python

## Core principle

Never call `python` or `python3` directly. Always use `uv run`.

Why: `uv run` automatically detects and uses the project's virtual environment, ensuring the correct dependencies are always in play. Calling `python` directly risks picking up the wrong global interpreter or a mismatched environment.

## Basic usage

### Running a script

```bash
# Don't do this
python script.py
python3 script.py

# Do this
uv run script.py
```

### Running a module

```bash
# Don't do this
python -m pytest
python3 -m uvicorn app:app

# Do this
uv run pytest           # if pytest is in pyproject.toml
uv run python -m uvicorn app:app
```

### Inline code

```bash
# Don't do this
python -c "import sys; print(sys.version)"

# Do this
uv run python -c "import sys; print(sys.version)"
```

## Using libraries temporarily (outside a project or without adding them)

When a library isn't in the project's `pyproject.toml`, or when working outside any project directory, use `--with` to add packages for that one run only.

```bash
# Use numpy temporarily
uv run --with numpy script.py

# Use multiple packages at once
uv run --with numpy --with pandas --with matplotlib script.py

# Works with inline code too
uv run --with httpx python -c "import httpx; print(httpx.get('https://example.com').status_code)"
```

Packages added via `--with` are not written to pyproject.toml — they exist only for that invocation. If you need the package permanently, use `uv add <package>` instead.

## Common scenarios

### Running tests

```bash
uv run pytest
uv run pytest tests/test_foo.py -v
uv run pytest --cov=src
```

### Starting a dev server

```bash
uv run uvicorn main:app --reload
uv run flask run
uv run manage.py runserver   # Django
```

### One-off data processing with a library not in the project

```bash
uv run --with pandas python -c "
import pandas as pd
df = pd.read_csv('data.csv')
print(df.describe())
"
```

## Quick reference

| Situation | Command |
|-----------|---------|
| Run a script | `uv run script.py` |
| Run a module | `uv run python -m module` or `uv run module` |
| Temporary package | `uv run --with package script.py` |
| Multiple temporary packages | `uv run --with pkg1 --with pkg2 script.py` |
