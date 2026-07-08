# DAY17: Checkpoint

**Closed book.** No looking back.

1. What does `T extends string ? "yes" : "no"` do when `T` is `number`?

2. What keyword lets you capture a type from a pattern (e.g., extracting `U` from `T[]`)?

3. What is distribution in conditional types, and how do you prevent it?

4. Write a conditional type that extracts the resolved value from a `Promise<T>`.

5. How would you filter a union to keep only function members?

---

<details>
<summary>Answers</summary>

1. Returns `"no"` because `number` does not extend `string`.
2. `infer` — used like `T extends (infer U)[] ? U : T`.
3. Distribution automatically applies the conditional to each member of a union. Prevent it by wrapping in `[T] extends [U]`.
4. `type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;`
5. `type FunctionMembers<T> = T extends (...args: any[]) => any ? T : never;`
</details>
