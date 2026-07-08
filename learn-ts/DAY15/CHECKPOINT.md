# DAY15: Checkpoint

**Closed book.** No looking back.

1. Given `interface Car { make: string; model: string; year: number }`, what is `keyof Car`?

2. What does `Person["age"]` give you at the type level?

3. What's the difference between runtime `typeof` and type-level `typeof`?

4. What does `T[keyof T]` compute?

5. How does `K extends keyof T` constrain a generic parameter?

---

<details>
<summary>Answers</summary>

1. `"make" | "model" | "year"`
2. The type of the `age` property — `number`.
3. Runtime `typeof` is a JavaScript operator that returns a string. Type-level `typeof` captures the TypeScript type of a value.
4. A union of all property value types in `T` — the union of `T["key1"] | T["key2"] | ...`.
5. It restricts `K` to only the valid property names of `T`. You can only pass actual keys of `T`.
</details>
