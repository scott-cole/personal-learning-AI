# DAY26 Checkpoint

## Questions

1. How does pytest discover test files and functions?
2. What does `pytest.raises(ValueError)` do?
3. What is the purpose of a `@pytest.fixture`?
4. How do you test a function with multiple different inputs and expected outputs?
5. How do you run pytest with a coverage report?

<details>
<summary>Answers</summary>

1. It auto-discovers files matching `test_*.py` or `*_test.py`, and functions matching `test_*` inside them.
2. It asserts that the code inside the `with pytest.raises(ValueError):` block raises a `ValueError`.
3. A fixture provides reusable setup/teardown logic that tests can depend on (e.g., creating test data, database connections).
4. Use `@pytest.mark.parametrize("input,expected", [(...), (...)])` above the test function.
5. `pytest --cov=module_name test_file.py`

</details>
