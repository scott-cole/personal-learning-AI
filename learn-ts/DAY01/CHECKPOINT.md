# DAY01: Checkpoint

**Closed book.** No looking back at the lesson. Answer in your own words.

1. What is the difference between `let` and `const` in TypeScript?

2. What error does TypeScript give for this code: `let name: string = "Scott"; name = 42;`?

3. You write `let price = 9.99`. What type does TypeScript infer for `price`?

4. Why would a senior engineer NEVER use `number` to store a price in a banking app without converting to cents first?

5. What command runs a TypeScript file directly without compiling first?

---

**Check your answers against the lesson after you finish.**

<details>
<summary>Answers</summary>

1. `const` cannot be reassigned after declaration. `let` can be reassigned.
2. `Type 'number' is not assignable to type 'string'`
3. `number`
4. Floating-point arithmetic is imprecise (`0.1 + 0.2 !== 0.3`). Store money as integer cents.
5. `npx tsx index.ts`
</details>
