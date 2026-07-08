# DAY21: Checkpoint

**Closed book.** No looking back.

1. What is the Repository pattern, and why is it useful?

2. Why does `Repository<T>` use `T extends { id: string }` instead of just `T`?

3. What does `Partial<T>` in the `update` method allow?

4. Why make repository methods `async` even if the implementation is synchronous in memory?

5. How would you swap from `InMemoryRepository` to a database without changing the code that uses it?

---

<details>
<summary>Answers</summary>

1. It abstracts data access behind a standard interface (CRUD). Callers don't care if data is in memory, a file, or a database — they just call `findAll()`, `create()`, etc.
2. To guarantee that `T` has an `id` property used by `findById`, `update`, and `delete`.
3. Callers can pass only the fields they want to change, without providing every property.
4. To satisfy the interface contract and prepare for real async implementations (database queries, file I/O, network calls).
5. Create a new class that implements `Repository<T>` (e.g., `PostgresRepository`), then change only the instantiation line — all usage code stays identical.
</details>
