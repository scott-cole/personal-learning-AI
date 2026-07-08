# DAY08 — Comprehensions & Generator Expressions

## Anecdote: Comprehensions are an assembly line

Walk through a car factory. On one side, a conveyor belt carries raw chassis frames. As each frame passes, robots weld doors, attach wheels, paint the body, and install seats. By the time it reaches the end, a bare frame has become a finished car — all in one continuous flow, no manual lifting between stations.

A list comprehension is that assembly line. You start with a sequence (the raw frames), apply a transformation (the robots), optionally filter (defective frames are shunted aside), and collect the output. The whole thing happens in a single, readable expression. No need to `for`-loop, `append`, and repeat.

Generator expressions are the same assembly line, but without the warehouse at the end. The cars drive off the line one at a time, as needed, instead of filling a lot.

---

## List comprehensions

Basic syntax: `[expression for item in iterable if condition]`

### Without filter

```python
# Traditional loop
squares = []
for x in range(10):
    squares.append(x**2)

# Comprehension
squares = [x**2 for x in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### With filter

```python
evens = [x for x in range(20) if x % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

### With transformation and filter

```python
names = ["Alice", "Bob", "Charlie", "Dave"]
short_upper = [n.upper() for n in names if len(n) <= 4]
# ['ALICE', 'BOB', 'DAVE']
```

### Nested loops

```python
pairs = [(x, y) for x in range(3) for y in range(3)]
# [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]
```

### `if/else` in the expression

```python
parity = ["even" if x % 2 == 0 else "odd" for x in range(5)]
# ['even', 'odd', 'even', 'odd', 'even']
```

---

## Dict comprehensions

```python
squares_dict = {x: x**2 for x in range(6)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Filtering
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
# {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Swapping keys and values
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}
# {1: 'a', 2: 'b', 3: 'c'}
```

---

## Set comprehensions

```python
vowels = {c for c in "hello world" if c in "aeiou"}
# {'e', 'o'}  — unordered, unique
```

---

## Generator expressions

Same syntax as list comprehensions, but with `()` instead of `[]`. They produce values **on demand** — one at a time — instead of building the entire list in memory.

```python
# List — builds everything
squares_list = [x**2 for x in range(1000000)]  # ~8 MB in memory

# Generator — produces on demand
squares_gen = (x**2 for x in range(1000000))    # ~200 bytes

print(next(squares_gen))  # 0
print(next(squares_gen))  # 1
print(next(squares_gen))  # 4
```

### When generators shine

```python
# Sum of first million squares — no need to store them all
total = sum(x**2 for x in range(1_000_000))

# Any, all
has_even = any(x % 2 == 0 for x in [1, 3, 5, 7, 8])

# Read lines from a file lazily
lines = (line.strip() for line in open("bigfile.txt"))
```

### Generator vs list comprehension — when to use

| Use case | Pick |
|----------|------|
| Need to iterate once, then discard | Generator |
| Need random access, `len()`, or multiple passes | List |
| Large dataset (memory-constrained) | Generator |
| Small dataset, readability | List |

---

## Performance comparison

```python
import timeit

# List comprehension: builds full list
timeit.timeit('[x**2 for x in range(1000)]', number=10000)  # ~0.5s

# Generator: no list built
timeit.timeit('sum(x**2 for x in range(1000))', number=10000)  # ~0.6s
```

The generator is slightly slower per iteration (lazy overhead) but uses far less memory.

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| List comprehension | `[expr for item in iterable if cond]` |
| Dict comprehension | `{k: v for item in iterable if cond}` |
| Set comprehension | `{expr for item in iterable if cond}` |
| Generator expression | `(expr for item in iterable if cond)` — lazy |
| Memory | Generators don't store the entire sequence |
| Iteration | Generators are single-use; lists are reusable |
