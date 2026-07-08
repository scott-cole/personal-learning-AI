# DAY02 Checkpoint

## Questions

1. What does a Python function return if it has no `return` statement?
2. What is the difference between a parameter and an argument?
3. What will `print(greet())` output given `def greet(name="World"): return f"Hello, {name}"`?
4. Why does `def append_to(item, target=[])` behave unexpectedly on the second call?
5. What does the LEGB rule describe?

<details>
<summary>Answers</summary>

1. `None`
2. A parameter is the variable in the function definition; an argument is the value passed when calling.
3. `Hello, World` — the default parameter is used.
4. Default arguments are evaluated once at definition time, so the same list is shared across all calls.
5. The order Python uses to look up names: Local, Enclosing, Global, Built-in.

</details>
