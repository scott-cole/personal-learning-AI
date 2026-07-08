# DAY08 Checkpoint

## Questions

1. What is the output of `[x * 2 for x in range(5) if x % 2 == 1]`?
2. How is a generator expression different from a list comprehension?
3. What does `{v: k for k, v in {"a": 1, "b": 2}.items()}` return?
4. True or False: You can iterate over a generator expression multiple times.
5. What is the result of `sum(x**2 for x in range(4))`?

<details>
<summary>Answers</summary>

1. `[2, 6]` — `x` values 1 and 3, doubled.
2. Generator expressions use `()` instead of `[]`, produce items lazily (on demand), and don't store the entire sequence in memory.
3. `{1: 'a', 2: 'b'}` — keys and values are swapped.
4. False — generator expressions are single-use; once exhausted, they cannot be restarted.
5. `14` — `0 + 1 + 4 + 9 = 14`

</details>
