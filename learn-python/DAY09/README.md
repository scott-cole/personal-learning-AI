# DAY09 — File I/O

## Anecdote: File I/O is a librarian

You walk into a library. At the front desk sits a librarian. You hand them a call slip (the *filename*), and they say "I need the right key to open that room" (the *mode* — read, write, append). They fetch the book, hand it to you, and you read at a table. When you're done, you return the book. The librarian locks the room behind you.

Python's file I/O works exactly like this. You `open()` a file (hand the slip to the librarian), do your reading or writing, and `close()` it (return the book). The `with` statement is like an automated book return — Python guarantees the file is closed even if something goes wrong.

---

## Opening files

```python
file = open("example.txt", "r")   # read mode (default)
content = file.read()
file.close()
```

### Modes

| Mode | What it does |
|------|-------------|
| `"r"` | Read (default). File must exist. |
| `"w"` | Write. Creates or **overwrites**. |
| `"a"` | Append. Creates if missing, writes at end. |
| `"r+"` | Read and write. File must exist. |
| `"x"` | Exclusive creation. Fails if file exists. |

Add `"b"` for binary mode: `"rb"`, `"wb"`.

---

## The `with` statement — context manager

Always use `with`. It closes the file automatically.

```python
with open("example.txt", "r") as f:
    content = f.read()
# File is closed here, even if an error occurred
```

---

## Reading files

```python
# Entire file as one string
with open("poem.txt", "r") as f:
    content = f.read()

# List of lines
with open("poem.txt", "r") as f:
    lines = f.readlines()

# Iterate line by line (memory-efficient for large files)
with open("poem.txt", "r") as f:
    for line in f:
        print(line.strip())
```

---

## Writing files

```python
# Write (overwrites)
with open("output.txt", "w") as f:
    f.write("Hello, World!\n")
    f.write("Second line\n")

# Append
with open("output.txt", "a") as f:
    f.write("Third line\n")

# Write multiple lines from a list
lines = ["line1", "line2", "line3"]
with open("output.txt", "w") as f:
    f.writelines(line + "\n" for line in lines)
```

---

## Context managers beyond files

Any object that implements `__enter__` and `__exit__` can be used with `with`.

```python
from contextlib import contextmanager

@contextmanager
def temporary_change(filename, new_content):
    """Replace file contents temporarily."""
    import os
    backup = None
    try:
        if os.path.exists(filename):
            with open(filename, "r") as f:
                backup = f.read()
        with open(filename, "w") as f:
            f.write(new_content)
        yield
    finally:
        if backup is not None:
            with open(filename, "w") as f:
                f.write(backup)

with temporary_change("config.txt", "debug=true"):
    # File has new content inside this block
    pass
# File is restored outside the block
```

---

## File paths

```python
import os

# Join paths safely
path = os.path.join("data", "subdir", "file.txt")

# Get absolute path
abs_path = os.path.abspath("file.txt")

# Check existence
print(os.path.exists("file.txt"))     # True/False
print(os.path.isfile("file.txt"))     # True/False
print(os.path.isdir("mydir"))         # True/False

# List directory contents
print(os.listdir("."))
```

### Pathlib (modern, Python 3.4+)

```python
from pathlib import Path

p = Path("data/subdir/file.txt")
print(p.read_text())          # Read file
p.write_text("new content")   # Write file
p.parent                      # data/subdir
p.name                        # file.txt
p.stem                        # file
p.suffix                      # .txt
```

---

## File encoding

Always specify encoding. UTF-8 is the standard.

```python
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `open()` | Returns a file object in the given mode |
| `with` | Context manager — auto-closes the file |
| `read()` / `readlines()` | Read entire file or lines |
| `write()` / `writelines()` | Write strings or list of strings |
| `os.path` / `pathlib` | Cross-platform file path handling |
| Encoding | Always use `encoding="utf-8"` |
| Modes | `r`, `w`, `a`, `x`, `r+` |
