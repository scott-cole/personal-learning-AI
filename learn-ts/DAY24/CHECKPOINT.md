# DAY24: Checkpoint

**Closed book.** No looking back.

1. What's the difference between `.parse()` and `.safeParse()`?

2. What does `z.infer<typeof MySchema>` give you?

3. Why is runtime validation (Zod) needed when you already have TypeScript?

4. Write a Zod schema for an object with `name: string (min 1)` and `age: number (0-150)`.

5. What does `result.error.flatten()` return?

---

<details>
<summary>Answers</summary>

1. `.parse()` returns the validated data or throws a `ZodError`. `.safeParse()` returns a discriminated union `{ success, data } | { success, error }` — no throw.
2. The TypeScript type that corresponds to the Zod schema. If the schema validates successfully, the data will match `z.infer<typeof MySchema>`.
3. TypeScript types are erased at runtime. Zod validates data at runtime when it arrives from APIs, user input, or file I/O — when you can't trust the types.
4. `z.object({ name: z.string().min(1), age: z.number().int().min(0).max(150) })`
5. An object with `fieldErrors` (per-field error arrays) and `formErrors` (top-level errors), making it easy to return validation details in an API response.
</details>
