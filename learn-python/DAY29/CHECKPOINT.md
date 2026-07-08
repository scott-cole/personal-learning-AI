# DAY29 Checkpoint

## Questions

1. What HTTP status code is typically used for redirects in a URL shortener?
2. How would you check if a URL has already been shortened to avoid duplicates?
3. What does `url_for("redirect_url", short_code="abc123", _external=True)` generate?
4. Why use `string.ascii_letters + string.digits` for short codes instead of just sequential integers?
5. What SQL query increments the visit count for a short URL?

<details>
<summary>Answers</summary>

1. `302 Found` (temporary redirect).
2. Query the database: `SELECT short_code FROM urls WHERE original_url = ?` — if a row exists, return the existing code instead of creating a new one.
3. A full absolute URL like `http://127.0.0.1:5000/abc123`.
4. Random codes are harder to guess than sequential IDs (no sequential enumeration), and they provide a large namespace (62^6 ≈ 56 billion possibilities).
5. `UPDATE urls SET visits = visits + 1 WHERE short_code = ?`

</details>
