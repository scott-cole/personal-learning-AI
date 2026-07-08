# DAY09: Checkpoint

**Closed book.** No looking back.

1. What does `typeof x === "string"` tell TypeScript about `x` in the `if` block?

2. Why can't `typeof` distinguish between an array and a plain object?

3. What is a discriminated union, and what makes it "discriminated"?

4. Given `enum Direction { North, South, East, West }`, what is `Direction.East`?

5. What narrowing technique would you use to check if a value is an instance of a custom class?

---

<details>
<summary>Answers</summary>

1. It narrows the type — inside the block, `x` is treated as `string`.
2. `typeof` returns `"object"` for both arrays and plain objects (and `null`).
3. A union type where each member has a common property (the "discriminant") with a literal type, like `{ kind: "circle" }` vs `{ kind: "square" }`.
4. `2` (enums default to zero-indexed numbers).
5. `instanceof` — `if (x instanceof MyClass)`.
</details>
