# DAY20 — Virtual Environments

## Anecdote: Virtual envs are a separate workbench

Your workshop has one main workbench. On it sits a project — a birdhouse. The tools you need are a hammer, a saw, and wood glue. But now a new project arrives — a bookshelf — which needs a drill, a level, and a different kind of glue. You could pile everything on one bench, but the tools would mix together, you'd grab the wrong glue, and the birdhouse saw might not work for the bookshelf cuts.

Instead, you pull up a *second* workbench. You stock it with exactly the tools the bookshelf needs. When the bookshelf is done, you can even fold the bench away. The birdhouse bench stays untouched, with its own tools neatly arranged.

A Python virtual environment is that separate workbench. Each project gets its own `site-packages` directory with its own dependencies. Packages installed for Project A never interfere with Project B. And when the project is done, you delete the environment without affecting the rest of your system.

---

## Why virtual environments?

- **Isolation** — Project A needs Flask 2.0, Project B needs Flask 3.0
- **Reproducibility** — You can freeze exact versions to share with teammates
- **Cleanliness** — Your system Python stays pristine
- **Safety** — No accidental `pip install` that breaks your OS tools

---

## Creating and using virtual environments

### Built-in: `venv`

```bash
# Create the environment
python3 -m venv venv

# Activate (macOS/Linux)
source venv/bin/activate

# Activate (Windows)
venv\Scripts\activate

# Your prompt now shows (venv) — indicating the environment is active

# Deactivate
deactivate
```

### What happens when you activate?

- `PATH` is modified so `python` and `pip` point to the venv's versions
- `sys.prefix` changes to the venv directory
- `pip install` puts packages into `venv/lib/python3.x/site-packages/`

---

## Managing packages

```bash
# Install
pip install requests

# Specific version
pip install flask==2.3.0

# Install from requirements
pip install -r requirements.txt

# List installed
pip list

# Export to requirements
pip freeze > requirements.txt
```

---

## `requirements.txt`

Standard format:

```
flask==2.3.0
requests>=2.28.0
numpy
pandas==1.5.3
```

Pin exact versions for reproducibility (`==`). Use `>=` for minimum versions in libraries.

---

## `pyproject.toml` (modern alternative)

In modern Python packaging, `pyproject.toml` replaces `setup.py` and often `requirements.txt`.

```toml
[build-system]
requires = ["setuptools>=68.0"]
build-backend = "setuptools.backends._legacy:_Backend"

[project]
name = "my-project"
version = "0.1.0"
dependencies = [
    "flask>=2.3",
    "requests>=2.28",
]

[project.optional-dependencies]
dev = [
    "pytest",
    "pytest-cov",
]
```

```bash
pip install -e .               # Install project in editable mode
pip install -e ".[dev]"        # Install with dev dependencies
```

---

## Working with multiple environments

```bash
# Create envs for different projects
cd ~/project-a
python3 -m venv venv
source venv/bin/activate
pip install flask==2.3

cd ~/project-b
python3 -m venv venv
source venv/bin/activate
pip install flask==3.0

# Switch between them
deactivate
cd ~/project-a
source venv/bin/activate
```

---

## Common workflow

```bash
# 1. Clone the repo
git clone https://github.com/user/project.git
cd project

# 2. Create and activate venv
python3 -m venv venv
source venv/bin/activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Work on the project
python main.py

# 5. Update requirements after adding packages
pip freeze > requirements.txt

# 6. Deactivate when done
deactivate
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `venv` | Built-in tool for creating isolated Python environments |
| Activation | `source venv/bin/activate` — modifies PATH |
| `pip install` | Installs packages into the active environment |
| `pip freeze` | Lists all installed packages with versions |
| `requirements.txt` | Shareable dependency list |
| `pyproject.toml` | Modern project metadata + dependencies |
| Isolation | Each project gets its own toolset |
