# DAY17 — Lambda, map, filter, reduce & sorted

## Anecdote: Lambda is a Post-it note function

You're in the office kitchen. Someone has stuck a handwritten note on the fridge: "Milk — 2% — buy more if less than half." That's a tiny, single-purpose instruction, scribbled in 30 seconds, stuck exactly where it's needed, and no one filed it in the company handbook.

A lambda is that Post-it note — a small, anonymous function written on the fly for a single use. You don't `def` it with a full name and docstring; you just scribble `lambda x: x * 2` and pass it directly to `map` or `sorted`. It's not meant to be reused or imported. It's meant to be there, in place, doing one tiny job.

---

## Lambda syntax

```python
lambda arguments: expression
```

```python
square = lambda x: x**2
print(square(5))          # 25

add = lambda a, b: a + b
print(add(3, 7))          # 10
```

Lambdas are limited to a single expression (no statements, no assignments). Use them where a small function is needed for a short time.

---

## `map(function, iterable)` — transform each item

```python
nums = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, nums))
print(squared)             # [1, 4, 9, 16, 25]

# Multiple iterables
a = [1, 2, 3]
b = [10, 20, 30]
sums = list(map(lambda x, y: x + y, a, b))
print(sums)                # [11, 22, 33]
```

`map()` returns an iterator (lazy). Wrap in `list()` to materialise.

---

## `filter(function, iterable)` — keep matching items

```python
nums = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, nums))
print(evens)               # [2, 4, 6]

# filter(None, iterable) — removes falsy values
values = [0, 1, "", "hello", None, [], [1]]
truthy = list(filter(None, values))
print(truthy)              # [1, "hello", [1]]
```

---

## `reduce(function, iterable)` — accumulate

`reduce()` is in the `functools` module.

```python
from functools import reduce

nums = [1, 2, 3, 4, 5]
product = reduce(lambda a, b: a * b, nums)
print(product)             # 120

# With initial value
total = reduce(lambda a, b: a + b, nums, 100)
print(total)               # 115

# Max using reduce
maximum = reduce(lambda a, b: a if a > b else b, nums)
print(maximum)             # 5
```

---

## `sorted()` with `key`

```python
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 72},
    {"name": "Charlie", "grade": 91},
    {"name": "Diana", "grade": 88},
]

# Sort by grade
by_grade = sorted(students, key=lambda s: s["grade"])
for s in by_grade:
    print(f"{s['name']}: {s['grade']}")
# Bob: 72, Alice: 85, Diana: 88, Charlie: 91

# Sort by grade descending
by_grade_desc = sorted(students, key=lambda s: s["grade"], reverse=True)

# Sort by name length
by_name_len = sorted(students, key=lambda s: len(s["name"]))

# Sort by multiple keys
by_grade_then_name = sorted(students, key=lambda s: (s["grade"], s["name"]))
```

---

## `min()` / `max()` with `key`

```python
words = ["apple", "banana", "cherry", "date"]
longest = max(words, key=len)
print(longest)             # banana

shortest = min(words, key=len)
print(shortest)            # date
```

---

## Lambda vs list comprehension

Many `map`/`filter` patterns are more readable as comprehensions:

```python
# map + filter with lambdas
result = list(map(lambda x: x**2, filter(lambda x: x % 2 == 0, range(10))))

# Comprehension (more readable)
result = [x**2 for x in range(10) if x % 2 == 0]

print(result)  # [0, 4, 16, 36, 64]
```

Use comprehensions by default. Use `map`/`filter` when you already have a named function:

```python
# Comprehensions are cleaner here:
squares = [x**2 for x in range(10)]

# map with a named function makes sense:
numbers = ["1", "2", "3"]
parsed = list(map(int, numbers))  # cleaner than [int(x) for x in numbers]
```

---

## Summary

| Function | Purpose | Example |
|----------|---------|---------|
| `lambda` | Anonymous single-expression function | `lambda x: x * 2` |
| `map()` | Transform each element | `map(str.upper, items)` |
| `filter()` | Keep matching elements | `filter(is_even, nums)` |
| `reduce()` | Accumulate to a single value | `reduce(mul, nums)` |
| `sorted()` | Return a sorted copy | `sorted(items, key=len)` |
| `min`/`max` | Extremum with key | `max(words, key=len)` |
