# DAY23 — Transactions: ACID Properties

## Anecdote: Transactions are a Bank Transfer

You're transferring $500 from your savings account to your checking account. This is a two-step operation:
1. Subtract $500 from savings
2. Add $500 to checking

What happens if the power goes out after step 1 but before step 2? You've lost $500. That's unacceptable.

A **transaction** wraps both steps into a single atomic unit. Either both steps happen, or neither happens. The database guarantees this even if the server catches fire mid-operation.

This is the "A" in **ACID** — Atomicity. Combined with Consistency, Isolation, and Durability, these four properties are what make databases reliable for critical data.

## BEGIN and COMMIT

```sql
BEGIN;  -- Start a transaction

UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;

COMMIT; -- Make changes permanent
```

If the server crashes between the two UPDATEs, the database rolls back the first one automatically on restart.

## ROLLBACK

```sql
BEGIN;

UPDATE accounts SET balance = balance - 500 WHERE id = 1;
-- Oops! I meant to transfer $300, not $500.
-- Let's undo everything since BEGIN.

ROLLBACK; -- All changes in this transaction are undone
```

## SAVEPOINT — Partial Rollback

```sql
BEGIN;

UPDATE accounts SET balance = 100 WHERE id = 1;
SAVEPOINT sp1;

UPDATE accounts SET balance = 200 WHERE id = 2;
-- Problem! Something went wrong.

ROLLBACK TO SAVEPOINT sp1;  -- Undo only the second update
-- id 1 is still 100, id 2 is back to original

COMMIT;
```

## ACID Properties

### Atomicity
All operations in a transaction succeed or all fail. No partial results.

### Consistency
A transaction brings the database from one valid state to another. Constraints and rules are preserved.

### Isolation
Concurrent transactions don't interfere with each other. PostgreSQL uses MVCC (Multi-Version Concurrency Control) to let readers read without blocking writers, and vice versa.

### Durability
Once COMMIT returns, the data is safely stored on disk — even if the power fails immediately after.

## Isolation Levels

PostgreSQL supports four isolation levels:

```sql
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;
-- Default in PostgreSQL. Sees only committed data.
-- Prevents dirty reads.

BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- Sees a snapshot of data as of the start of the transaction.

BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
-- Highest level. Transactions execute as if they ran one after another.
```

## Deadlocks

When two transactions each wait for the other to release a lock:

```sql
-- Transaction 1:
BEGIN; UPDATE accounts SET balance = 100 WHERE id = 1;
-- Transaction 2:
BEGIN; UPDATE accounts SET balance = 200 WHERE id = 2;
-- Transaction 1:
UPDATE accounts SET balance = 300 WHERE id = 2;  -- Waits for T2
-- Transaction 2:
UPDATE accounts SET balance = 400 WHERE id = 1;  -- Waits for T1
-- DEADLOCK! PostgreSQL detects and kills one transaction.
```

## Auto-Commit

Most PostgreSQL clients (including psql) have auto-commit ON by default — every statement runs in its own transaction. You need to explicitly type `BEGIN` to start a multi-statement transaction.

## Summary

| Command | Purpose |
|---------|---------|
| `BEGIN` | Start a transaction |
| `COMMIT` | Make all changes permanent |
| `ROLLBACK` | Undo all changes since BEGIN |
| `SAVEPOINT name` | Set a partial rollback point |
| `ROLLBACK TO name` | Roll back to a savepoint |
| `RELEASE name` | Remove a savepoint |
