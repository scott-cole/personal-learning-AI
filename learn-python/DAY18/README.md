# DAY18 — Context Managers

## Anecdote: Context managers are a hotel room

You arrive at a hotel. The front desk gives you a key card. You enter the room, use it, sleep, shower. When you leave, you return the key card. The hotel *guarantees* the room will be cleaned before the next guest. You don't have to worry about changing the sheets or emptying the bins — the `with` block (your stay) ensures setup (check-in) and teardown (check-out) happen automatically.

A context manager is exactly that: it guarantees that resources are acquired before your code runs and released after — even if your code raises an exception. The `with` statement is your hotel stay.

---

## The `with` statement

```python
with open("file.txt", "r") as f:
    content = f.read()
# File is closed here, no matter what
```

Without `with`:

```python
f = open("file.txt", "r")
try:
    content = f.read()
finally:
    f.close()
```

`with` makes this cleaner and eliminates the risk of forgetting to close.

---

## Writing your own context manager — class-based

Implement `__enter__` and `__exit__`.

```python
class ManagedFile:
    def __init__(self, filename, mode="r"):
        self.filename = filename
        self.mode = mode

    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file  # what gets bound to `as f`

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        # Return False to propagate exceptions, True to suppress
        return False

with ManagedFile("hello.txt", "w") as f:
    f.write("Hello, context manager!")
```

### `__exit__` parameters

| Parameter | Meaning |
|-----------|---------|
| `exc_type` | The exception class (e.g., `ValueError`), or `None` |
| `exc_val` | The exception instance, or `None` |
| `exc_tb` | The traceback object, or `None` |

```python
class SuppressErrors:
    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type is not None:
            print(f"Suppressed: {exc_val}")
        return True  # Suppress the exception

with SuppressErrors():
    x = 1 / 0  # Normally crashes — but it's suppressed
print("Still running!")
```

---

## `contextlib` utilities

### `@contextmanager` decorator

Turns a generator function into a context manager. Code before `yield` is `__enter__`; code after is `__exit__`.

```python
from contextlib import contextmanager

@contextmanager
def managed_file(filename, mode):
    file = open(filename, mode)
    try:
        yield file
    finally:
        file.close()

with managed_file("hello.txt", "w") as f:
    f.write("Hello, decorator!")
```

### `contextlib.closing()`

Ensures an object's `.close()` method is called. Useful for objects that don't implement `__enter__`.

```python
from contextlib import closing
import urllib

with closing(urllib.urlopen("https://python.org")) as page:
    content = page.read()
```

### `contextlib.suppress()`

Suppress specific exceptions (Python 3.4+).

```python
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove("maybe_not_exist.txt")  # No error if it doesn't exist
```

### `contextlib.nullcontext()`

A no-op context manager — useful as a placeholder.

```python
from contextlib import nullcontext

def process(need_lock=False):
    ctx = threading.Lock() if need_lock else nullcontext()
    with ctx:
        # Critical section or not, depending on need_lock
        pass
```

---

## Practical example: timing

```python
import time
from contextlib import contextmanager

@contextmanager
def timer(label="Block"):
    start = time.perf_counter()
    try:
        yield
    finally:
        elapsed = time.perf_counter() - start
        print(f"{label} took {elapsed:.4f}s")

with timer("Sleepy"):
    time.sleep(0.5)
# Sleepy took 0.5001s
```

---

## Nesting context managers

```python
with open("source.txt", "r") as src, open("dest.txt", "w") as dst:
    dst.write(src.read())
```

For many contexts (Python 3.1+):

```python
from contextlib import ExitStack

filenames = ["a.txt", "b.txt", "c.txt"]
with ExitStack() as stack:
    files = [stack.enter_context(open(f, "r")) for f in filenames]
    # All files are open. They will all be closed when we exit the with block.
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `with` | Automates setup and teardown |
| `__enter__` | Acquire resource, return value for `as` |
| `__exit__` | Release resource, handle exceptions |
| `@contextmanager` | Generator-based context managers |
| `contextlib.suppress` | Suppress specific exceptions |
| `contextlib.closing` | Ensure `.close()` is called |
| `ExitStack` | Manage dynamic number of contexts |
