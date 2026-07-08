# DAY03 — Strings

## Anecdote: Strings are beads on a string

Picture a jewellery maker's table. A length of thread lies across the workbench, and beside it is a bowl of colourful beads — each bead etched with a single character. The maker can slide beads onto the thread, count them, snip the thread at any point, grab a section from the middle, or flip the whole necklace around. The beads themselves never change — you cannot un-etch a bead — but you can always string new ones together.

A Python string is that thread of beads. Characters are the beads. You can access any position, slice out a substring, or create new strings by joining. But you cannot change a character *in place* — strings are **immutable**.

---

## Creating strings

```python
single = 'hello'
double = "hello"
triple = """Multi-line
string right here"""
```

Single and double quotes are interchangeable. Triple quotes preserve newlines.

---

## Indexing

Each character has a position — an **index**. Python is zero-based.

```python
word = "Python"
#       012345   (positive indices)
#      ...-3-2-1 (negative indices)

print(word[0])    # P
print(word[2])    # t
print(word[-1])   # n
print(word[-3])   # h
```

Accessing an out-of-range index raises `IndexError`.

---

## Slicing

`s[start:stop:step]` extracts a substring.

```python
text = "Hello, World!"

print(text[0:5])      # Hello   (indices 0 to 4)
print(text[7:])       # World!  (7 to end)
print(text[:5])       # Hello   (start to 4)
print(text[::2])      # Hlo ol! (every other)
print(text[::-1])     # !dlroW ,olleH  (reversed)
```

- `start` is inclusive, `stop` is exclusive
- Omitting `start` defaults to `0`, omitting `stop` defaults to the end

---

## Immutability

```python
name = "Ada"
# name[0] = "E"      # TypeError: 'str' object does not support item assignment
name = "Eda"         # This creates a *new* string and reassigns the variable
```

---

## Common string methods

Methods return *new* strings — they never modify the original.

```python
msg = "  python is FUN!  "

print(msg.upper())          #   PYTHON IS FUN!
print(msg.lower())          #   python is fun!
print(msg.strip())          # "python is FUN!"
print(msg.replace("FUN", "awesome"))  #   python is awesome!

tags = "red, green, blue"
print(tags.split(", "))     # ['red', 'green', 'blue']

parts = ["2024", "01", "15"]
print("-".join(parts))      # 2024-01-15

print("hello".startswith("he"))   # True
print("hello".endswith("lo"))     # True
print("hello".count("l"))         # 2
print("42".isdigit())             # True
```

---

## f-strings (formatted string literals)

```python
name = "Marie"
age = 28
print(f"{name} is {age} years old.")
# Marie is 28 years old.
```

### Expression evaluation

```python
print(f"{2 ** 10}")           # 1024
print(f"{name.upper()}")      # MARIE
```

### Format specifiers

```python
pi = 3.14159265
print(f"{pi:.2f}")            # 3.14
print(f"{pi:10.2f}")          # "      3.14"  (width 10)
print(f"{pi:010.2f}")         # 0000003.14

val = 42
print(f"{val:d}")             # 42
print(f"{val:5d}")            # "   42"
print(f"{val:05d}")           # 00042
```

### Alignment

```python
print(f"{'left':<10}|")       # left      |
print(f"{'center':^10}|")     #   center  |
print(f"{'right':>10}|")      #      right|
```

---

## Escape sequences

| Sequence | Meaning |
|----------|---------|
| `\n` | Newline |
| `\t` | Tab |
| `\\` | Backslash |
| `\'` | Single quote |
| `\"` | Double quote |

```python
print("Line1\nLine2")
# Line1
# Line2

print("He said \"Hello\"")
# He said "Hello"
```

Raw strings ignore escape sequences:

```python
path = r"C:\Users\name"
print(path)       # C:\Users\name
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Strings | Immutable sequences of characters |
| Indexing | Zero-based; negative counts from end |
| Slicing | `[start:stop:step]` — start inclusive, stop exclusive |
| Methods | Return new strings; originals unchanged |
| f-strings | Embed expressions with `{}` and format specifiers |
| Escape seqs | `\n`, `\t`, `\\`, etc. |
| Raw strings | `r"..."` — ignore backslash escapes |
