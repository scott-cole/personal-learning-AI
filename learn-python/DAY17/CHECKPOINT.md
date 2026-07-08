# DAY17 Checkpoint

## Questions

1. What is a lambda function limited to (one expression, multiple statements, or both)?
2. What does `map(int, ["1", "2", "3"])` return?
3. What is the result of `reduce(lambda a, b: a if a > b else b, [3, 7, 2, 9, 5])`?
4. How would you sort a list of strings by their length using `sorted()`?
5. Which is generally more readable: `map`/`filter` with lambdas or list comprehensions?

<details>
<summary>Answers</summary>

1. A single expression (no statements, no assignments).
2. An iterator yielding integers `1`, `2`, `3` — wrap with `list()` to materialise.
3. `9` — it finds the maximum value.
4. `sorted(strings, key=len)`
5. List comprehensions are generally more readable for simple transformations and filters.

</details>
