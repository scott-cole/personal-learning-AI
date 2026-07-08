# DAY01 — Variables & Types

## Anecdote: Variables are labelled boxes on a shelf

Imagine a tidy workshop. On a wooden shelf sit cardboard boxes, each with a sticky note slapped on the front. The sticky note is the *variable name*. Inside the box is the *value*. You can peek inside, replace the contents, or even stick a new label on. But the box *itself* — the container — is what your program reaches for when it sees the name.

Python is the same. When you write `age = 25`, you are grabbing a fresh box, writing "age" on the label, and stuffing the integer `25` inside. Later, when you type `age`, Python runs to the shelf and hands you whatever is in that box.

---

## Variables

A variable is created the moment you assign a value to it.

```python
name = "Marie"
score = 100
pi_approx = 3.14
is_ready = True
```

No declaration keyword needed. No type annotation required (though Python 3 supports optional hints). The variable's type is inferred from the value you put in the box.

### Naming rules
- Must start with a letter or underscore (`_`)
- Remaining characters can be letters, digits, or underscores
- Case-sensitive: `Age` and `age` are two different boxes
- Cannot be a reserved keyword (`if`, `for`, `def`, `class`, etc.)

```python
# Valid
user_name = "Alice"
_counter = 0
speed2 = 50

# Invalid — would crash
# 2fast = "no"
# my-var = "no"
# class = "no"
```

---

## Basic Types

| Type | Example | Description |
|------|---------|-------------|
| `int` | `42`, `-7`, `0` | Whole numbers (unlimited precision) |
| `float` | `3.14`, `-2.5`, `1e3` | Decimal numbers (IEEE 754 double) |
| `str` | `"hello"`, `'world'` | Text, Unicode, immutable |
| `bool` | `True`, `False` | Logical values (subclass of `int`) |

```python
a = 10          # int
b = 3.14        # float
c = "Python"    # str
d = True        # bool

print(type(a))  # <class 'int'>
print(type(b))  # <class 'float'>
print(type(c))  # <class 'str'>
print(type(d))  # <class 'bool'>
```

### Dynamic typing

The same variable can hold different types over its lifetime (but don't do this — it confuses everyone).

```python
x = 5          # x is int
x = "hello"    # x is now str — same box, new contents
```

---

## The `type()` function

`type()` returns the type of any object. It's your X-ray goggles for peeking into boxes.

```python
print(type(42))         # <class 'int'>
print(type(3.14))       # <class 'float'>
print(type("text"))     # <class 'str'>
print(type(False))      # <class 'bool'>
print(type([1, 2]))     # <class 'list'>
```

---

## The `print()` function

`print()` writes values to the console. You can pass multiple items separated by commas, and change the separator or end character.

```python
print("Hello, World!")              # Hello, World!
print("Score:", 95)                 # Score: 95
print("a", "b", "c", sep=", ")     # a, b, c
print("loading", end="...\n")       # loading...
```

---

## Type conversion

Sometimes you need to move a value from one kind of box to another.

```python
# str → int
age = int("25")

# int → str
msg = "You are " + str(25) + " years old"

# float → int (truncates!)
n = int(3.99)       # 3

# anything → bool
bool(0)             # False
bool(42)            # True
bool("")            # False
bool("hello")       # True
```

---

## f-strings (preview)

We dive deep into strings on Day 3, but f-strings are too useful to hide.

```python
name = "Ada"
year = 1815
print(f"{name} was born in {year}.")
# Ada was born in 1815.
```

The `f` before the opening quote tells Python to evaluate expressions inside `{}`.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Variables | Labelled boxes on a shelf — names that hold values |
| Dynamic typing | The box holds anything; the label says nothing about contents |
| `int` / `float` | Numbers — whole and decimal |
| `str` | Text — immutable sequence of characters |
| `bool` | `True` / `False` — yes or no |
| `type()` | Goggles that reveal what's in the box |
| `print()` | Shouts the contents to the console |
| f-strings | Embed expressions in strings with `{}` |
