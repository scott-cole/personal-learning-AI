# DAY11: Checkpoint

**Closed book.** No looking back.

1. What does `async` before a function declaration do?

2. What type does `async function foo(): Promise<number>` return if the function body returns `42`?

3. What's the difference between sequential `await` calls and `Promise.all`?

4. What happens if one promise in `Promise.all` rejects?

5. Write an async function that fetches two users by id and returns their names using `Promise.all`.

---

<details>
<summary>Answers</summary>

1. It makes the function return a Promise automatically and allows `await` inside it.
2. A `Promise<number>` that resolves to `42`.
3. Sequential awaits run one after another (total time = sum of each). `Promise.all` runs them concurrently (total time = slowest one).
4. The entire `Promise.all` rejects immediately with that error (fail-fast).
5. `async function getUsers(): Promise<string[]> { const [u1, u2] = await Promise.all([fetchUser(1), fetchUser(2)]); return [u1.name, u2.name]; }`
</details>
