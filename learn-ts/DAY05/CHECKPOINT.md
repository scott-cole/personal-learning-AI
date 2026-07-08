# DAY05: Checkpoint

**Closed book.** No looking back.

1. What does the `|` symbol mean in `string | number`?

2. What's wrong with `let dir: "north" | "south" = "up";`?

3. What is a discriminated union?

4. Given `type ID = string | number`, why doesn't TypeScript prevent you from assigning an invalid UUID string to `ID`?

5. Write a union type `StatusCode` that allows only `200`, `404`, or `500` as values.

---

<details>
<summary>Answers</summary>

1. "Or" — the value can be either a string or a number.
2. `"up"` is not in the allowed literal values (`"north"` or `"south"`).
3. A union type where each member has a common property (the "discriminant") with a literal type, so you can narrow by checking that property.
4. `ID` is just an alias for `string | number`. TypeScript doesn't enforce string format — it only checks the type. Use branded types or Zod for that.
5. `type StatusCode = 200 | 404 | 500;`
</details>
