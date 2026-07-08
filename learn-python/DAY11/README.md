# DAY11 — Modules & Packages

## Anecdote: Modules are toolboxes; PyPI is a hardware store

Your garage workshop has a wall of toolboxes. One toolbox contains only wrenches. Another has screwdrivers. A third has pliers. When you need to tighten a bolt, you don't dump all three boxes on the floor — you grab the wrench box, pull out the 10mm wrench, and put the box back.

Python modules are those toolboxes. Each module is a `.py` file containing related functions and classes. `math` is the maths toolbox. `random` is the randomness toolbox. `json` is the data-serialisation toolbox.

Now imagine a hardware store the size of a supermarket — aisle after aisle of every tool imaginable, built by people around the world. That's **PyPI** (the Python Package Index). `pip` is your shopping cart.

---

## Importing modules

```python
# Import the whole module
import math
print(math.sqrt(16))      # 4.0

# Import specific items
from math import sqrt, pi
print(sqrt(16))           # 4.0
print(pi)                 # 3.141592653589793

# Import with alias
import numpy as np
import pandas as pd

# Import all (avoid — pollutes namespace)
from math import *
```

---

## Creating your own module

Any `.py` file is a module. Create `helpers.py`:

```python
# helpers.py
def greet(name):
    return f"Hello, {name}!"

PI = 3.14159
```

Use it from another file in the same directory:

```python
# main.py
import helpers
print(helpers.greet("Ada"))     # Hello, Ada!
print(helpers.PI)               # 3.14159
```

---

## `if __name__ == "__main__"`

This guard lets a file be both importable and runnable.

```python
# calculator.py
def add(a, b):
    return a + b

def sub(a, b):
    return a - b

if __name__ == "__main__":
    # This only runs when you execute `python3 calculator.py`
    print("Testing calculator...")
    print(f"add(3, 5) = {add(3, 5)}")
```

When you import `calculator`, the test code does not run. When you execute it directly, it does.

---

## Packages — directories of modules

A package is a directory with an `__init__.py` file (can be empty).

```
my_package/
    __init__.py
    math_ops.py
    string_ops.py
```

```python
# my_package/math_ops.py
def double(x):
    return x * 2
```

```python
from my_package.math_ops import double
print(double(5))  # 10
```

The `__init__.py` can set what gets imported with `from package import *` via `__all__`.

---

## Standard library highlights

```python
import os           # Operating system interface
import sys          # System-specific parameters
import json         # JSON encoder/decoder
import csv          # CSV file reading/writing
import re           # Regular expressions
import math         # Mathematical functions
import random       # Random number generation
import datetime     # Date and time
import collections  # Specialized data structures
import itertools    # Iterator tools
import pathlib      # Object-oriented filesystem paths
```

---

## `pip` and PyPI

```bash
# Install a package
pip install requests

# Install a specific version
pip install requests==2.31.0

# Install from requirements file
pip install -r requirements.txt

# List installed packages
pip list

# Show package info
pip show requests

# Uninstall
pip uninstall requests
```

---

## `requirements.txt`

Standard format for listing dependencies:

```
requests==2.31.0
flask>=2.0
numpy
pandas
```

Generate from your environment:

```bash
pip freeze > requirements.txt
```

---

## The module search path

Python looks for modules in:

1. The directory of the running script
2. Directories in `PYTHONPATH` environment variable
3. Standard library directories
4. Site-packages (where `pip` installs)

```python
import sys
print(sys.path)  # List of all search directories
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `import` | Load a module into your namespace |
| `from ... import ...` | Import specific names |
| `__name__ == "__main__"` | Guard against import-side effects |
| Packages | Directories of modules with `__init__.py` |
| `pip` | Package installer for PyPI |
| `requirements.txt` | List of dependencies for a project |
| Standard library | Batteries included — use before installing third-party |
