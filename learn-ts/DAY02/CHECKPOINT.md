# DAY02: Checkpoint

**Closed book.** No looking back.

1. What does `: string` mean in `function greet(name: string): string` — both positions?

2. Write an arrow function that takes a `number` and returns its square, using implicit return.

3. What's the difference between an optional parameter (`?`) and a default parameter?

4. What return type does a function that only calls `console.log` have?

5. Why does `greet(42)` fail if `greet` expects `name: string`?

---

<details>
<summary>Answers</summary>

1. `name: string` means the parameter must be a string. The second `: string` after the parens means the function returns a string.
2. `const square = (x: number): number => x * x;`
3. Optional params are `undefined` if not provided. Default params use a fallback value instead of `undefined`.
4. `void`
5. `42` is a number, not a string. TypeScript catches the type mismatch before runtime.
</details>
