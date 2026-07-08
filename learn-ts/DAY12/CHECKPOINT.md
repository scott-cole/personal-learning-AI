# DAY12: Checkpoint

**Closed book.** No looking back.

1. What's the difference between `throw` and `return`?

2. Why is the error in `catch (error)` typed as `unknown` in modern TypeScript?

3. What does the `finally` block guarantee?

4. How does the `Result<T>` pattern differ from throwing exceptions?

5. Write a `safeDivide` that returns a `Result<number>` instead of throwing.

---

<details>
<summary>Answers</summary>

1. `throw` unwinds the stack and jumps to the nearest `catch` block. `return` hands a value back to the caller normally.
2. Because you could throw anything (string, number, Error), so TypeScript doesn't assume it's an Error.
3. It always executes, whether the try block succeeds, throws, or returns — used for cleanup.
4. Result makes errors explicit in the return type. The caller *must* check `.ok` before using `.value`. Throwing is implicit — callers can forget try/catch.
5. `function safeDivide(a: number, b: number): Result<number> { if (b === 0) return { ok: false, error: "Division by zero" }; return { ok: true, value: a / b }; }`
</details>
