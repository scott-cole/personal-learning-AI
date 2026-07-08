# DAY18 Drill — The Hotel Manager

## Task

Write a Python script (`drill.py`) with two context managers, then demonstrate them.

### 1. `Timer` context manager (class-based)

```python
class Timer:
    """Context manager that prints how long the block took."""
    def __init__(self, label="Block"):
        self.label = label

    def __enter__(self):
        self.start = time.perf_counter()
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.elapsed = time.perf_counter() - self.start
        print(f"{self.label}: {self.elapsed:.4f}s")
        return False  # don't suppress exceptions
```

### 2. `atomic_write` context manager (`@contextmanager`)

Write data to a temp file first, then rename — so if the write fails, the original file is untouched.

```python
from contextlib import contextmanager

@contextmanager
def atomic_write(filename, mode="w"):
    """Write atomically using a temporary file."""
    import tempfile, os
    tmp = tempfile.NamedTemporaryFile(mode=mode, delete=False, dir=".")
    try:
        yield tmp
        tmp.close()
        os.replace(tmp.name, filename)
    except:
        tmp.close()
        os.unlink(tmp.name)
        raise
```

### 3. Demonstrate both

- Use `Timer` to time a `time.sleep(0.3)` call
- Use `atomic_write` to safely write a file
- Verify the file was written

### Expected output

```
=== TIMER ===
Before sleep
After sleep
Sleepy block: 0.3XXs
=== ATOMIC WRITE ===
Writing to output.txt atomically...
Written content: Hello from atomic write!
Content verified: Hello from atomic write!
```

### Hints

- `time.perf_counter()` for precision timing
- `os.replace()` is atomic on most OS
- `NamedTemporaryFile(delete=False)` prevents auto-deletion
- Read the file back with `Path("output.txt").read_text()` to verify

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Implement `Timer` class with `__enter__`/`__exit__`
- [ ] Implement `atomic_write` using `@contextmanager`
- [ ] Demonstrate `Timer` with `time.sleep()`
- [ ] Demonstrate `atomic_write` to safely write a file
- [ ] Verify the file content
