# DAY26 Drill — The Safety Net

## Task

Write a Python module and comprehensive tests for it using pytest.

### Step 1: Create `calculator.py`

```python
# calculator.py

def add(a, b):
    return a + b

def sub(a, b):
    return a - b

def mul(a, b):
    return a * b

def div(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b

def factorial(n):
    if not isinstance(n, int) or n < 0:
        raise ValueError("n must be a non-negative integer")
    if n <= 1:
        return 1
    return n * factorial(n - 1)

def is_palindrome(s):
    cleaned = s.lower().replace(" ", "")
    return cleaned == cleaned[::-1]
```

### Step 2: Create `test_calculator.py`

Write tests that cover:

1. **Basic arithmetic** — test `add`, `sub`, `mul`, `div` with normal values
2. **Division by zero** — use `pytest.raises(ZeroDivisionError)`
3. **Factorial** — test `factorial(0)`, `factorial(1)`, `factorial(5)`, and invalid input
4. **Palindrome** — use `@pytest.mark.parametrize` to test at least 5 cases
5. **Fixtures** — Use a fixture that provides a list of test numbers
6. **Temporary file** — Use `tmp_path` to test writing calculator results to a file

### Expected output

```
=== RUNNING TESTS ===
$ pytest test_calculator.py -v

test_calculator.py::test_add PASSED
test_calculator.py::test_sub PASSED
test_calculator.py::test_mul PASSED
test_calculator.py::test_div PASSED
test_calculator.py::test_div_by_zero PASSED
test_calculator.py::test_factorial_zero PASSED
test_calculator.py::test_factorial_one PASSED
test_calculator.py::test_factorial_five PASSED
test_calculator.py::test_factorial_invalid PASSED
test_calculator.py::test_is_palindrome[racecar-True] PASSED
test_calculator.py::test_is_palindrome[hello-False] PASSED
... (all parametrized cases) PASSED
test_calculator.py::test_with_fixture PASSED
test_calculator.py::test_write_results PASSED
```

### Hints

- `@pytest.mark.parametrize("s,expected", [("racecar", True), ("hello", False), ...])`
- Fixture: `@pytest.fixture` returning `[1, 2, 3, 4, 5]`
- `tmp_path / "results.txt"` for the file test
- Run with `pytest -v` to see each test name

### How to run

```bash
pip install pytest pytest-cov
pytest test_calculator.py -v
pytest --cov=calculator test_calculator.py  # with coverage
```

## TODOs

- [ ] Create `calculator.py` with functions
- [ ] Write tests for basic arithmetic
- [ ] Test exception cases
- [ ] Use parametrize for palindrome tests
- [ ] Create a fixture
- [ ] Use `tmp_path` for file I/O test
- [ ] Run tests with coverage
