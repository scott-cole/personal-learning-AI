# DAY21: Week 3 Cumulative Checkpoint

**Closed book. Write SQL from memory.**

1. Write a CREATE TABLE statement for a `visits` table that tracks visits to short URLs:
   - `id` — auto-incrementing integer
   - `code` — text, foreign key to urls
   - `visited_at` — timestamp
   - `ip_address` — text, nullable

2. Write a query that returns the top 5 most-visited URLs.

3. What's the difference between `INNER JOIN` and `LEFT JOIN`?

4. Write a Go function that wraps an error with context:
   ```go
   func saveURL(db *sql.DB, url URL) error {
       _, err := db.Exec("INSERT INTO urls ...")
       // wrap the error here
   }
   ```

5. What does `defer tx.Rollback()` do and why is it safe after `tx.Commit()`?

6. Write a migration that adds a `description` column to the `urls` table.
