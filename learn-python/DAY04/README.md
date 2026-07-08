# DAY04 — Lists

## Anecdote: A list is a numbered locker bank

Walk into a gym changing room. Along the wall is a bank of lockers — each with a number stencilled on the door: locker 0, locker 1, locker 2, and so on. You can stuff anything into a locker: a towel, a phone, a sandwich, even another locker number written on a scrap of paper. You can peer into a locker, swap its contents, or pop things out. The lockers always stay in order. If you remove locker 5, all the lockers to the right slide left to fill the gap.

A Python list is that row of lockers. Each locker is an *element*, and its number is the *index*. The list *remembers the order* of everything you put in.

---

## Creating lists

```python
empty = []
numbers = [1, 2, 3, 4, 5]
mixed = [42, "hello", 3.14, True]
nested = [[1, 2], [3, 4]]
```

---

## Indexing and slicing

Same zero-based indexing and `[start:stop:step]` slicing as strings.

```python
items = ["apple", "banana", "cherry", "date", "elderberry"]

print(items[0])        # apple
print(items[-1])       # elderberry
print(items[1:3])      # ['banana', 'cherry']
print(items[::-1])     # reversed list
```

Unlike strings, lists are **mutable** — you can change elements in place.

```python
items[1] = "blueberry"
print(items)           # ['apple', 'blueberry', 'cherry', 'date', 'elderberry']
```

---

## Common list methods

```python
fruits = ["apple", "banana"]

# Add to the end
fruits.append("cherry")          # ['apple', 'banana', 'cherry']

# Insert at a position
fruits.insert(1, "blueberry")    # ['apple', 'blueberry', 'banana', 'cherry']

# Remove and return the last item (or at an index)
last = fruits.pop()              # 'cherry'
print(fruits)                    # ['apple', 'blueberry', 'banana']

first = fruits.pop(0)            # 'apple'

# Remove by value (first occurrence)
fruits.remove("banana")          # removes 'banana'

# Find index of a value
fruits.index("blueberry")        # 0

# Count occurrences
[1, 2, 2, 3].count(2)           # 2

# Sort in place
nums = [3, 1, 4, 1, 5]
nums.sort()                      # [1, 1, 3, 4, 5]
nums.sort(reverse=True)          # [5, 4, 3, 1, 1]

# Reverse in place
nums.reverse()

# Clear all items
nums.clear()                     # []
```

---

## `len()`, `min()`, `max()`, `sum()`

```python
nums = [10, 20, 30, 40, 50]
print(len(nums))       # 5
print(min(nums))       # 10
print(max(nums))       # 50
print(sum(nums))       # 150
```

---

## Looping over a list

```python
for fruit in fruits:
    print(fruit)

# With index
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

---

## List concatenation and repetition

```python
a = [1, 2, 3]
b = [4, 5, 6]
print(a + b)           # [1, 2, 3, 4, 5, 6]
print(a * 3)           # [1, 2, 3, 1, 2, 3, 1, 2, 3]
```

---

## Checking membership

```python
print("banana" in fruits)     # True
print("grape" not in fruits)  # True
```

---

## Copying lists

Assignment just copies the reference — both variables point to the same lockers.

```python
original = [1, 2, 3]
alias = original         # same list!
alias[0] = 99
print(original[0])       # 99

# Shallow copy
copy1 = original.copy()
copy2 = original[:]
copy3 = list(original)
```

---

## Nested lists

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

print(matrix[1][2])      # 6
```

---

## List destructuring (unpacking)

```python
first, second, third = [10, 20, 30]
print(first)    # 10

head, *tail = [1, 2, 3, 4]
print(head)     # 1
print(tail)     # [2, 3, 4]
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Lists | Ordered, mutable collections |
| Indexing | Zero-based; negative counts from end |
| Methods | `.append()`, `.pop()`, `.sort()`, `.reverse()`, etc. |
| Mutable | Elements can be changed in place |
| Copying | Use `.copy()`, `[:]`, or `list()` to copy |
| Nesting | Lists can contain any type, including other lists |
| Unpacking | `a, b, c = [1, 2, 3]` |
