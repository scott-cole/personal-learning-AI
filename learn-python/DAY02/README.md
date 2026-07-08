# DAY02 — Functions

## Anecdote: A function is a blender

You own a blender. You drop in bananas, milk, and ice (the *inputs*). You press a button. The blades spin, motors whir (the *body*). Out comes a smoothie (the *output*). You don't care *how* the blades spin — you just want the smoothie. That's a function.

You can give the blender a recipe card (the *definition*). As long as the kitchen has the blender, anyone can walk up, drop in ingredients, and get a smoothie without knowing anything about motors or blades.

In Python, `def` is your blender's ON switch.

---

## Defining a function

```python
def greet(name):
    """Say hello to someone."""
    return f"Hello, {name}!"
```

| Part | What it is |
|------|-----------|
| `def` | Keyword that starts the definition |
| `greet` | Function name — use lowercase with underscores |
| `(name)` | Parameter list — the inputs your blender expects |
| `:` | Colon marks the start of the function body |
| `"""..."""` | Docstring — describes what the function does |
| `return` | Sends the result back to the caller |

### Calling a function

```python
message = greet("Ada")
print(message)          # Hello, Ada!
```

When you call `greet("Ada")`, Python:
1. Pauses whatever it was doing
2. Jumps into the function body
3. Binds `name = "Ada"`
4. Runs the code inside
5. Returns the result and resumes

---

## Parameters vs arguments

- **Parameter**: the variable listed in the function definition (`name`)
- **Argument**: the actual value you pass when calling (`"Ada"`)

```python
def add(a, b):       # a and b are parameters
    return a + b

result = add(3, 5)   # 3 and 5 are arguments
```

---

## Positional and keyword arguments

```python
def describe_pet(name, species, age):
    return f"{name} is a {age}-year-old {species}"

# Positional — order matters
print(describe_pet("Fido", "dog", 3))

# Keyword — order doesn't matter
print(describe_pet(species="cat", age=2, name="Whiskers"))

# Mix — positionals first, then keywords
print(describe_pet("Bella", species="parrot", age=5))
```

---

## Default parameters

```python
def repeat(msg, times=3):
    return (msg + " ") * times

print(repeat("hi"))          # hi hi hi
print(repeat("go", 2))       # go go
```

Default parameters are evaluated once at definition time — use `None` for mutable defaults.

```python
def bad_append(item, target=[]):
    target.append(item)
    return target

print(bad_append(1))         # [1]
print(bad_append(2))         # [1, 2] — oops! shared list

def good_append(item, target=None):
    if target is None:
        target = []
    target.append(item)
    return target
```

---

## Return values

A function without a `return` statement returns `None`.

```python
def say_hi():
    print("hi!")          # prints, but returns None

result = say_hi()         # hi!
print(result)             # None
```

You can return multiple values as a tuple:

```python
def min_max(nums):
    return min(nums), max(nums)

low, high = min_max([3, 1, 7, 2])
print(low, high)          # 1 7
```

---

## Docstrings

Triple-quoted strings right after `def` are docstrings. They become the function's `__doc__` attribute.

```python
def factorial(n):
    """Return n! (n factorial) for non-negative integers.

    >>> factorial(5)
    120
    """
    if n <= 1:
        return 1
    return n * factorial(n - 1)

print(factorial.__doc__)
```

---

## Scope — the LEGB rule

Variables inside a function are *local* — they don't leak out.

```python
x = 10          # global

def func():
    y = 5       # local
    print(x)    # 10 — can read globals
    print(y)    # 5

func()
print(y)        # NameError — y doesn't exist out here
```

To *write* to a global variable from inside a function, use `global` (avoid when possible).

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| `def` | Define a reusable block of code |
| Parameters | Inputs your function accepts |
| Arguments | Actual values passed when calling |
| `return` | Sends a value back to the caller |
| Docstrings | Triple-quoted documentation strings |
| Scope | Variables inside functions stay local |
| Defaults | Use `None` for mutable default parameters |
