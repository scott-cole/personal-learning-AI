# DAY06 — Conditionals & Loops

## Anecdote: Conditionals are a fork in the road

You're hiking a forest trail. You reach a signpost: left goes to the waterfall, right goes to the summit. You check the weather (the *condition*). If it's sunny, you take the summit trail; if it's raining, you head to the waterfall; otherwise, you sit on the bench and eat a sandwich.

That fork in the trail is an `if/elif/else` block. Your program reaches a decision point, evaluates a single boolean expression, and follows exactly one branch. No looking back.

Loops, on the other hand, are a **treadmill**. You step on, and the belt keeps moving underneath you until you tell it to stop. Each step is one *iteration*. The `for` loop walks through a fixed number of steps; the `while` loop keeps going until you jump off.

---

## Conditionals: `if` / `elif` / `else`

```python
temperature = 30

if temperature > 35:
    print("Too hot!")
elif temperature > 25:
    print("Warm day")
elif temperature > 15:
    print("Pleasant")
else:
    print("Chilly")
```

- Python uses indentation (4 spaces) to define blocks
- No parentheses needed around conditions
- `elif` is short for "else if"
- `else` is optional
- At most one branch executes

### Truthiness

Values that evaluate to `False` in a boolean context:

```python
bool(0)         # False
bool(0.0)       # False
bool("")        # False
bool([])        # False
bool({})        # False
bool(None)      # False
```

Everything else is `True`.

### Chained comparisons

```python
x = 5
print(1 < x < 10)      # True
print(1 < x < 4)       # False
```

### Ternary expression

```python
age = 20
status = "adult" if age >= 18 else "minor"
```

---

## `for` loops — iteration over a sequence

```python
# List
for fruit in ["apple", "banana", "cherry"]:
    print(fruit)

# String
for char in "Python":
    print(char)

# Dictionary
person = {"name": "Ada", "age": 28}
for key in person:
    print(key, person[key])

for key, value in person.items():
    print(key, value)

# Set
for item in {1, 2, 3}:
    print(item)
```

---

## `range()` — generate sequences of numbers

```python
# range(stop)
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

# range(start, stop)
for i in range(2, 6):     # 2, 3, 4, 5
    print(i)

# range(start, stop, step)
for i in range(0, 10, 2): # 0, 2, 4, 6, 8
    print(i)

# Backwards
for i in range(10, 0, -1):  # 10, 9, ..., 1
    print(i)
```

---

## `enumerate()` — loop with index

```python
words = ["zero", "one", "two"]
for index, word in enumerate(words):
    print(f"{index}: {word}")
# 0: zero
# 1: one
# 2: two
```

---

## `while` loops

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

### `break` — exit the loop early

```python
for n in range(100):
    if n == 5:
        break
    print(n)             # 0, 1, 2, 3, 4
```

### `continue` — skip to the next iteration

```python
for n in range(10):
    if n % 2 == 0:
        continue
    print(n)             # 1, 3, 5, 7, 9
```

### `else` on loops

The `else` block runs only if the loop finished normally (no `break`).

```python
for n in range(5):
    if n == 10:
        break
else:
    print("Loop completed without break")  # this runs

for n in range(5):
    if n == 3:
        break
else:
    print("This won't run")
```

---

## Nested loops

```python
for i in range(3):
    for j in range(3):
        print(f"({i},{j})", end=" ")
    print()

# (0,0) (0,1) (0,2)
# (1,0) (1,1) (1,2)
# (2,0) (2,1) (2,2)
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `if/elif/else` | Exactly one branch executes |
| Truthiness | `0`, `""`, `[]`, `None`, etc. are falsy |
| `for` | Iterates over a sequence |
| `range()` | Generates integer sequences |
| `enumerate()` | Loops with index and value |
| `while` | Loops until a condition is false |
| `break` / `continue` | Exit early / skip iteration |
| Loop `else` | Runs if no `break` occurred |
