# DAY15 — Decorators

## Anecdote: Decorators are a gift wrapper

You've bought a present — a plain cardboard box. Before handing it over, you wrap it in shiny paper, add a ribbon, and attach a tag. The gift *inside* is still the same, but now it has wrapping that adds delight (or a "do not open until Christmas" instruction). The wrapping doesn't change what the gift *is* — it changes how it's *presented*.

A decorator is gift wrap for a function. It takes your function, wraps it in extra behaviour (logging, timing, authentication, caching), and returns the wrapped version. Your original function stays unchanged, but now every call passes through the wrapping.

---

## Functions are objects

In Python, functions are first-class objects — you can pass them around, assign them to variables, and return them from other functions.

```python
def greet(name):
    return f"Hello, {name}!"

say_hi = greet          # no parentheses — assign, don't call
print(say_hi("Ada"))    # Hello, Ada!
```

---

## A decorator is a function that takes a function and returns a function

```python
def shout(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper() + "!!!"
    return wrapper

def greet(name):
    return f"Hello, {name}"

# Manual wrapping
greet = shout(greet)
print(greet("Ada"))     # HELLO, ADA!!!
```

---

## The `@` syntax

The `@decorator` syntax is just syntactic sugar for `func = decorator(func)`.

```python
def shout(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result.upper() + "!!!"
    return wrapper

@shout
def greet(name):
    return f"Hello, {name}"

print(greet("Ada"))     # HELLO, ADA!!!
```

---

## Common use cases

### Timing

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(0.5)
    return "Done"

slow_function()  # slow_function took 0.5001s
```

### Logging

```python
def log_calls(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}({args}, {kwargs})")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_calls
def add(a, b):
    return a + b

add(3, 5)
# Calling add((3, 5), {})
# add returned 8
```

### Access control

```python
def require_auth(func):
    def wrapper(user, *args, **kwargs):
        if not user.get("authenticated"):
            raise PermissionError("User not authenticated")
        return func(user, *args, **kwargs)
    return wrapper

@require_auth
def view_profile(user):
    return f"Profile: {user['name']}"
```

---

## `functools.wraps`

Without `@wraps`, the decorated function loses its identity (`__name__`, `__doc__`, etc.).

```python
import functools

def my_decorator(func):
    @functools.wraps(func)   # preserves func's metadata
    def wrapper(*args, **kwargs):
        print("Before")
        result = func(*args, **kwargs)
        print("After")
        return result
    return wrapper

@my_decorator
def say_hi():
    """Says hello."""
    return "hi!"

print(say_hi.__name__)   # say_hi (not wrapper)
print(say_hi.__doc__)    # Says hello.
```

Always use `@functools.wraps` when writing decorators.

---

## Decorators with arguments

```python
def repeat(times):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    return f"Hello, {name}"

print(greet("Ada"))
# ['Hello, Ada!', 'Hello, Ada!', 'Hello, Ada!']
```

---

## Multiple decorators

Stacked decorators apply bottom-up.

```python
@shout
@timer
def compute():
    return "result"

# Equivalent to: compute = shout(timer(compute))
```

---

## Summary

| Concept | Key Takeaway |
|---------|-------------|
| Decorator | A function that takes a function and returns a wrapped version |
| `@` syntax | Syntactic sugar for `func = decorator(func)` |
| `*args, **kwargs` | Accept and forward any arguments |
| `functools.wraps` | Preserves original function metadata |
| Stacking | Decorators apply bottom-up |
| Use cases | Timing, logging, auth, caching, validation |
