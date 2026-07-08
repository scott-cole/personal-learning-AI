# DAY05 — Dictionaries, Tuples & Sets

## Anecdote: Dicts are a coat check; sets are a VIP guest list

Picture a nightclub entrance. You hand your coat to the attendant, and they give you a stamped ticket with a number. Later, you hand back the ticket, and they fetch *your* coat — not someone else's. That's a dictionary: a **key** (the ticket number) mapped to a **value** (the coat). Fast, direct, no order needed.

Now picture the bouncer's clipboard at the VIP door. The clipboard has a list of names. There are no duplicates — you're either on the list or you're not. That's a set. The bouncer doesn't care *where* you are on the list; they just check membership. "Is Scott on the list?" — yes or no, instantly.

Tuples? Those are like a **museum display case** — a fixed arrangement of items behind glass. You can look, but you cannot touch.

---

## Dictionaries (`dict`)

Key-value pairs, unordered (insertion order preserved in Python 3.7+).

```python
# Creating
person = {
    "name": "Ada",
    "age": 28,
    "city": "London",
}

# Accessing
print(person["name"])        # Ada
print(person.get("name"))    # Ada
print(person.get("job"))     # None (no KeyError)
print(person.get("job", "Unknown"))  # "Unknown"

# Adding / updating
person["email"] = "ada@example.com"
person["age"] = 29

# Checking keys
print("name" in person)      # True

# Removing
removed = person.pop("city")
del person["age"]

# Keys, values, items
print(person.keys())        # dict_keys(['name', 'email'])
print(person.values())      # dict_values(['Ada', 'ada@example.com'])
print(person.items())       # dict_items([('name', 'Ada'), ('email', 'ada@example.com')])

# Looping
for key, value in person.items():
    print(f"{key}: {value}")
```

### Dictionary comprehensions

```python
squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

---

## Tuples (`tuple`)

Immutable sequences. Use them for fixed collections of related values.

```python
# Creating
point = (3, 4)
single = (1,)       # trailing comma needed
without_parens = 1, 2, 3

# Accessing — same as lists
print(point[0])     # 3

# Unpacking
x, y = point
print(x, y)         # 3 4

# Immutable
# point[0] = 5      # TypeError!

# Nested tuples
matrix = ((1, 2), (3, 4))
print(matrix[0][1]) # 2
```

### When to use tuples

- Return multiple values from a function
- Dictionary keys (lists can't be keys; tuples can if they contain only hashable types)
- Data that shouldn't change (coordinates, RGB colours, config constants)

---

## Sets (`set`)

Unordered collections of **unique**, **hashable** elements.

```python
# Creating
fruits = {"apple", "banana", "cherry"}
empty_set = set()     # NOT {} — that's a dict

# Adding / removing
fruits.add("date")
fruits.remove("banana")    # KeyError if missing
fruits.discard("banana")   # No error if missing
fruits.pop()               # Removes and returns an arbitrary element

# Membership — fast!
print("apple" in fruits)   # True

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)          # Union:     {1, 2, 3, 4, 5, 6}
print(a & b)          # Intersection: {3, 4}
print(a - b)          # Difference:   {1, 2}
print(a ^ b)          # Symmetric diff: {1, 2, 5, 6}

# Subset / superset
print({1, 2}.issubset(a))    # True
print(a.issuperset({1, 2}))  # True
```

### Set comprehensions

```python
evens = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
```

---

## When to use what

| Need | Choice |
|------|--------|
| Map keys to values | `dict` |
| Ordered sequence, mutable | `list` |
| Ordered sequence, immutable | `tuple` |
| Unique items, fast membership | `set` |

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `dict` | Key-value mapping — `{"key": value}` |
| `tuple` | Immutable, ordered — `(1, 2, 3)` |
| `set` | Unordered, unique — `{1, 2, 3}` |
| Membership | `in` works on all three |
| Unpacking | Works with tuples, lists, and dict items |
| Comprehension | `{k: v for ...}`, `{x for ...}` |
