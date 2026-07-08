# DAY15 Checkpoint

## Questions

1. What is a decorator in Python?
2. Why is `@functools.wraps` important when writing a decorator?
3. What is the output of stacking `@a` and `@b` on a function `f`? (i.e., `@a @b def f(): ...`)
4. How would you write a decorator that accepts arguments like `@repeat(3)`?
5. What does the `wrapper` function typically do with `*args, **kwargs`?

<details>
<summary>Answers</summary>

1. A decorator is a function that takes another function and returns a replacement (wrapped) function, adding behaviour without modifying the original.
2. It preserves the original function's `__name__`, `__doc__`, and other metadata so the decorated function is not anonymous.
3. It's equivalent to `a(b(f))` — decorators apply from bottom to top (closest to the function first).
4. With a factory: `def repeat(times): def decorator(func): ... return decorator`
5. It forwards them to the original function: `return func(*args, **kwargs)`.

</details>
