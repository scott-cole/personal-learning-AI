# DAY25: Checkpoint

**Closed book.** No looking back.

1. What does `it()` / `test()` represent in Vitest?

2. What's the difference between `toBe()` and `toEqual()`?

3. How do you test that a function throws an error?

4. How would you test an async function?

5. What does `vi.fn()` create?

---

<details>
<summary>Answers</summary>

1. A single test case. The first argument is a description, the second is a function containing assertions.
2. `toBe()` uses strict equality (`===`) — works for primitives. `toEqual()` does deep equality — works for objects and arrays.
3. Wrap the call in a function: `expect(() => fn()).toThrow()`.
4. Make the test callback `async` and `await` the function: `it("works", async () => { const result = await asyncFn(); expect(result).toBe(...); })`.
5. A mock function that records calls and can be configured with a return value or implementation.
</details>
