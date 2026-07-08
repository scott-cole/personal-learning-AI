# DAY10 Checkpoint

## Questions

1. What is the difference between `except:` and `except Exception as e:`?
2. When does the `else` block in a `try/except/else/finally` structure execute?
3. How do you create a custom exception called `ValidationError`?
4. What happens if you `raise` an exception inside a `try` block and no matching `except` exists?
5. What does `assert 1 == 0, "message"` do?

<details>
<summary>Answers</summary>

1. Bare `except:` catches everything including `KeyboardInterrupt` and `SystemExit`; `except Exception:` catches only non-fatal exceptions.
2. The `else` block runs only if no exception was raised in the `try` block.
3. `class ValidationError(Exception): pass`
4. The exception propagates up the call stack — if unhandled, it crashes the program.
5. It raises `AssertionError` with the message `"message"` because the condition is `False`.

</details>
