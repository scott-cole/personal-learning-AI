# DAY23 — Checkpoint

## Questions

**1. What does ACID stand for?**

<details>
<summary>Answer</summary>
Atomicity, Consistency, Isolation, Durability.
</details>

**2. What is the difference between COMMIT and ROLLBACK?**

<details>
<summary>Answer</summary>
`COMMIT` makes all changes in the transaction permanent; `ROLLBACK` undoes all changes.
</details>

**3. What is a SAVEPOINT used for?**

<details>
<summary>Answer</summary>
It creates a partial rollback point within a transaction, allowing you to undo changes after the savepoint without aborting the entire transaction.
</details>

**4. What is the default isolation level in PostgreSQL?**

<details>
<summary>Answer</summary>
`READ COMMITTED`.
</details>

**5. If a transaction has three UPDATE statements and the third one fails, what happens to the first two?**

<details>
<summary>Answer</summary>
If `COMMIT` is not reached, the entire transaction is rolled back. If the error is handled (within a PL/pgSQL block), the first two updates might persist depending on error handling. In a basic psql session, the transaction is aborted and must be rolled back.
</details>
