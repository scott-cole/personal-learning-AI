# DAY23 — Drill: Transaction Practice

## Setup

```sql
CREATE TABLE accounts (
    id SERIAL PRIMARY KEY,
    owner VARCHAR(100) NOT NULL,
    balance NUMERIC(10,2) NOT NULL DEFAULT 0
);

INSERT INTO accounts (owner, balance) VALUES
('Alice', 1000.00),
('Bob', 500.00),
('Charlie', 250.00);
```

## Tasks

### 1. Successful transfer

Write a transaction that transfers $200 from Alice to Bob. Use BEGIN, two UPDATEs, and COMMIT.

### 2. Rollback on error

Write a transaction that transfers $9999 from Bob to Charlie (more than Bob has). Don't check for insufficient funds — just run it. Then ROLLBACK. Verify the balances are unchanged.

### 3. Savepoint practice

```sql
BEGIN;
UPDATE accounts SET balance = balance + 100 WHERE id = 1;
SAVEPOINT after_alice;
UPDATE accounts SET balance = balance - 50 WHERE id = 2;
-- Decide to undo Bob's change but keep Alice's
ROLLBACK TO SAVEPOINT after_alice;
COMMIT;
```

Check the final balances.

### 4. Explicit locking

```sql
BEGIN;
SELECT balance FROM accounts WHERE id = 1 FOR UPDATE;
-- This locks Alice's row until COMMIT or ROLLBACK
-- (Open a second psql session and try to update Alice — it will wait)
COMMIT;
```

### 5. Test atomicity

```sql
BEGIN;
INSERT INTO accounts (owner, balance) VALUES ('Diana', 300);
INSERT INTO accounts (owner, balance) VALUES ('Eve', 'not-a-number');  -- This will fail
COMMIT;
```

What happens? Is Diana in the database?

## Hints

- Always COMMIT or ROLLBACK — leaving a transaction open holds locks
- Use `FOR UPDATE` to lock rows explicitly
- Run task 5 to see atomicity in action: the failed INSERT causes the entire transaction to abort
