# DAY16 Checkpoint

## Questions

1. What is the difference between `return` and `yield` in a function?
2. What happens when you call `next()` on an exhausted iterator?
3. How does a generator pipeline save memory compared to using lists?
4. What does `yield from` do?
5. True or False: A generator can be iterated over multiple times.

<details>
<summary>Answers</summary>

1. `return` exits the function and returns a value once; `yield` suspends the function, returns a value, and can be resumed later.
2. It raises `StopIteration`.
3. Generators produce items one at a time on demand, so only one item exists in memory at any moment instead of the entire sequence.
4. `yield from` delegates iteration to another iterable, yielding each element from it.
5. False — generators are single-use; once exhausted, they cannot be restarted.

</details>
