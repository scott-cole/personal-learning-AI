# DAY15 Drill — The Gift Wrapping Station

## Task

Write a Python script (`drill.py`) with three decorators and demonstrate them.

### Requirements

1. **`@log_calls`** — logs the function name, arguments, and return value to the console
2. **`@timer`** — prints how long the function took
3. **`@memoize`** — caches results for a function based on its arguments (so repeated calls with the same args return instantly)

### Decorator implementations

```python
import functools
import time

def log_calls(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"[LOG] Calling {func.__name__}({args}, {kwargs})")
        result = func(*args, **kwargs)
        print(f"[LOG] {func.__name__} returned {result}")
        return result
    return wrapper

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"[TIMER] {func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

def memoize(func):
    cache = {}
    @functools.wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
            print(f"[MEMO] Computing {func.__name__}{args}")
        else:
            print(f"[MEMO] Cache hit {func.__name__}{args}")
        return cache[args]
    return wrapper
```

### Functions to decorate

- `add(a, b)` — decorated with `@log_calls` and `@timer`
- `slow_double(n)` — decorated with `@timer`, sleeps for 0.5s, returns `n * 2`
- `fib(n)` — decorated with `@memoize`, returns the nth Fibonacci number (recursive)

### Expected output (approximate)

```
=== LOG + TIMER ===
[LOG] Calling add((3, 5), {})
[LOG] add returned 8
[TIMER] add took 0.0000s
Result: 8
=== TIMER ===
[TIMER] slow_double took 0.5001s
double 10 = 20
[TIMER] slow_double took 0.5002s
double 20 = 40
=== MEMOIZED FIB ===
[MEMO] Computing fib(35)
...fib(35) = 9227465
[MEMO] Cache hit fib(35)
...fib(35) = 9227465
Duration: ~0.0s (second call way faster)
```

### Hints

- Stack decorators: `@log_calls` above `@timer` applies `log_calls` *outside* `timer`
- The `fib` function should be recursive: `return 1 if n <= 2 else fib(n-1) + fib(n-2)`
- Use `time.perf_counter()` for precision timing

### How to run

```bash
python3 drill.py
```

## TODOs

- [ ] Implement `@log_calls` decorator
- [ ] Implement `@timer` decorator
- [ ] Implement `@memoize` decorator
- [ ] Decorate and test `add`, `slow_double`, `fib`
- [ ] Verify metadata preservation with `functools.wraps`
