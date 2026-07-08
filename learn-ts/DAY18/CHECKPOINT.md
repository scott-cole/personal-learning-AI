# DAY18: Checkpoint

**Closed book.** No looking back.

1. What does `Partial<User>` do to `User { id: number; name: string }`?

2. What's the difference between `Pick<User, "id" | "name">` and `Omit<User, "email">`?

3. When would you use `Record<string, number>` instead of a plain object type?

4. What does `NonNullable<string | null | undefined>` evaluate to?

5. Write a type `CreateTodoPayload` using utility types, assuming `Todo` has `id`, `title`, `completed`, `createdAt`.

---

<details>
<summary>Answers</summary>

1. Makes all properties optional: `{ id?: number; name?: string }`.
2. `Pick` keeps only the listed keys. `Omit` keeps everything except the listed keys. They are opposites.
3. When you want a dictionary/map where all values have the same type and keys follow a pattern (especially with literal key types).
4. `string`
5. `type CreateTodoPayload = Omit<Todo, "id" | "createdAt">;`
</details>
