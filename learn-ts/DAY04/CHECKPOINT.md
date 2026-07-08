# DAY04: Checkpoint

**Closed book.** No looking back.

1. What's the difference between `number[]` and `[number, number]`?

2. What does `[1, 2, 3].map(x => x * 2)` return?

3. How would you merge two arrays `a` and `b` into a new array without mutating either?

4. Write a tuple type that represents a `[latitude, longitude]` coordinate pair.

5. What does `[1, 2, 3, 4, 5].filter(x => x > 3).reduce((a, b) => a + b, 0)` return?

---

<details>
<summary>Answers</summary>

1. `number[]` is an array of any length containing numbers. `[number, number]` is a tuple of exactly two numbers.
2. `[2, 4, 6]`
3. `const merged = [...a, ...b];`
4. `[number, number]`
5. `9` (4 + 5)
</details>
