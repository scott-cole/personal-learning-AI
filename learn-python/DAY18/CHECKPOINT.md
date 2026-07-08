# DAY18 Checkpoint

## Questions

1. What two methods must a class implement to be used as a context manager with `with`?
2. What does `__exit__` receive, and what does returning `True` mean?
3. How does the `@contextmanager` decorator from `contextlib` work?
4. What is the benefit of using `atomic_write` over plain `open()` for writing files?
5. What does `contextlib.suppress(FileNotFoundError)` do?

<details>
<summary>Answers</summary>

1. `__enter__` and `__exit__`.
2. It receives `(exc_type, exc_val, exc_tb)`. Returning `True` suppresses any exception; returning `False` (or `None`) lets it propagate.
3. It decorates a generator function — code before `yield` runs on enter, code after runs on exit in a `finally` block.
4. It writes to a temp file first; if the write fails, the original file is untouched. Only on success is the temp file atomically renamed to the target.
5. It swallows `FileNotFoundError` if raised inside the block — no crash if the file doesn't exist.

</details>
