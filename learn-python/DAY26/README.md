# DAY26 — Testing with pytest

## Anecdote: Tests are a safety net (revisited, but different)

A trapeze artist rehearses a new trick — a triple somersault. She doesn't start 30 feet in the air without a net. First, she practices on the ground (unit tests). Then with a low net (integration tests). Finally, with the full rig in performance (production). The net doesn't prevent falls — it makes falls *safe*.

Tests don't prevent bugs. They make bugs *visible* and *contained*. A good test suite gives you the confidence to refactor, add features, and deploy without fear. When you break something, you know immediately, and you fix it before it reaches a user.

---

## Installing pytest

```bash
pip install pytest pytest-cov
```

---

## Your first test

Create `test_example.py`:

```python
def test_addition():
    assert 2 + 2 == 4

def test_subtraction():
    assert 5 - 3 == 2
```

```bash
pytest
# ======================== 2 passed in 0.02s ========================
```

---

## Assertions

`pytest` uses plain `assert` statements — no special `self.assertEqual()` needed.

```python
def test_strings():
    assert "hello".upper() == "HELLO"
    assert "hello".startswith("he")
    assert len("hello") == 5

def test_lists():
    assert [1, 2, 3] == [1, 2, 3]
    assert 3 in [1, 2, 3]

def test_floats():
    # Use approx for floating-point comparisons
    assert 0.1 + 0.2 == pytest.approx(0.3)

def test_exceptions():
    with pytest.raises(ValueError):
        int("not_a_number")
```

---

## Fixtures — reusable test setup

```python
import pytest

@pytest.fixture
def sample_data():
    """Create test data that multiple tests can use."""
    return {"name": "Alice", "scores": [85, 92, 78]}

def test_average(sample_data):
    scores = sample_data["scores"]
    assert sum(scores) / len(scores) == 85.0

def test_name(sample_data):
    assert sample_data["name"] == "Alice"
```

Fixtures can also set up and tear down (database connections, temp files):

```python
@pytest.fixture
def temp_file():
    # Setup
    import tempfile, pathlib
    f = tempfile.NamedTemporaryFile(suffix=".txt", delete=False)
    f.write(b"hello")
    f.close()
    yield pathlib.Path(f.name)
    # Teardown
    f.unlink()

def test_read_file(temp_file):
    assert temp_file.read_text() == "hello"
```

---

## Parametrized tests — test many cases

```python
import pytest

def is_even(n):
    return n % 2 == 0

@pytest.mark.parametrize("input_val,expected", [
    (2, True),
    (3, False),
    (0, True),
    (-2, True),
    (-3, False),
])
def test_is_even(input_val, expected):
    assert is_even(input_val) == expected
```

---

## Testing with `tmp_path` (built-in fixture)

```python
def test_file_operations(tmp_path):
    d = tmp_path / "subdir"
    d.mkdir()
    f = d / "test.txt"
    f.write_text("content")
    assert f.read_text() == "content"
    assert f.stat().st_size == 7
```

---

## Running tests

```bash
# Run all tests in current directory
pytest

# Verbose
pytest -v

# Run a specific test file
pytest test_calculator.py

# Run a specific test function
pytest test_calculator.py::test_add

# Run tests matching a pattern
pytest -k "calc"

# Stop on first failure
pytest -x

# Show print() output
pytest -s

# With coverage report
pytest --cov=my_module
```

---

## Organizing tests

```
project/
├── src/
│   ├── calculator.py
│   └── database.py
└── tests/
    ├── test_calculator.py
    └── test_database.py
```

Run all tests with `pytest` from the project root.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `assert` | Simple, readable test assertions |
| `pytest` | Test runner — auto-discovers `test_*.py` files |
| Fixtures | `@pytest.fixture` — reusable setup/teardown |
| Parametrization | `@pytest.mark.parametrize` — test many cases |
| `tmp_path` | Built-in fixture for temporary files |
| `pytest.raises()` | Assert that an exception is raised |
| `pytest.approx()` | Fuzzy float comparisons |
| Coverage | `pytest --cov=module` |
