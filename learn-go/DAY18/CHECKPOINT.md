# DAY18: Checkpoint

**Closed book.**

1. What's `sql.ErrNoRows` and why do we map it to a domain error?

2. The `Service` doesn't need to change when you swap `MemoryRepository` for `SQLRepository`. Why?

3. Write the SQL for `Save(url storage.URL) error` in `SQLRepository`.

4. Write the SQL + Scan code for `FindByCode(code string)`.

5. What would happen if you called `repo.Save(url)` twice with the same code?
