# DAY20 Drill — The Separate Workbench

## Task

Set up a virtual environment, install packages, and create a requirements file for a small project.

### Step 1: Create the project structure

```bash
mkdir drill_project
cd drill_project
```

### Step 2: Create `calculator.py`

A simple calculator that doesn't need extra packages yet:

```python
def add(a, b): return a + b
def sub(a, b): return a - b
def mul(a, b): return a * b
def div(a, b): return a / b

if __name__ == "__main__":
    print("Calculator loaded. Run tests with pytest.")
```

### Step 3: Create the virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 4: Install a testing package

```bash
pip install pytest
```

### Step 5: Create `test_calculator.py`

```python
from calculator import add, sub, mul, div

def test_add():
    assert add(2, 3) == 5

def test_sub():
    assert sub(10, 4) == 6

def test_mul():
    assert mul(3, 4) == 12

def test_div():
    assert div(10, 2) == 5.0
```

### Step 6: Run tests

```bash
pytest test_calculator.py
```

**Expected output**: All 4 tests pass.

### Step 7: Freeze dependencies

```bash
pip freeze > requirements.txt
```

### Step 8: Create `run.sh` / `run.bat` (optional, for reference)

A small script showing the activation + test sequence.

### Expected `requirements.txt`

```
pytest==8.x.x
```

### What to turn in

Write a Python script (`drill.py`) inside `drill_project/` that:

1. Prints the Python executable path (`import sys; print(sys.executable)`)
2. Prints `sys.prefix` to confirm it's within the venv
3. Prints the installed packages using `pip` via `import subprocess`

Run this script from within the activated venv.

### Expected output (when venv is active)

```
Python executable: /path/to/drill_project/venv/bin/python
sys.prefix: /path/to/drill_project/venv
Installed packages:
Package    Version
---------- -------
pip        xx.x.x
pytest     8.x.x
```

### Hints

- `sys.executable` shows the current Python interpreter path
- `sys.prefix` shows the environment root
- Use `import subprocess; subprocess.run([sys.executable, "-m", "pip", "list"])` to show packages

### How to run

```bash
cd drill_project
source venv/bin/activate
python3 drill.py
```

## TODOs

- [ ] Create project directory and calculator module
- [ ] Create virtual environment
- [ ] Install pytest
- [ ] Write and run tests
- [ ] Freeze requirements
- [ ] Write drill.py to verify environment
