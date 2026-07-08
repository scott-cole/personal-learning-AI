# DAY19: Transactions — "All or Nothing"

---

## The anecdote: "A bank transfer"

You send £100 from your account to a friend's account. Two operations:

1. Debit £100 from you
2. Credit £100 to your friend

What if step 1 succeeds but step 2 fails? The money disappears. That's a bug.

A **transaction** says: "Both happen, or neither happens."

```
BEGIN TRANSACTION
  Debit £100 from account A
  Credit £100 to account B
COMMIT
```

If either step fails, `ROLLBACK` undoes everything.

---

## Transactions in Go

```go
tx, err := db.Begin()
if err != nil {
    return err
}
defer tx.Rollback() // safe — no-op if already committed

_, err = tx.Exec("UPDATE accounts SET balance = balance - ? WHERE id = ?", 100, accountA)
if err != nil {
    return err // defers to Rollback
}

_, err = tx.Exec("UPDATE accounts SET balance = balance + ? WHERE id = ?", 100, accountB)
if err != nil {
    return err // defers to Rollback
}

err = tx.Commit()
if err != nil {
    return err
}
// tx.Rollback() in defer is now a no-op
```

**Pattern:** `tx, err := db.Begin()` then `defer tx.Rollback()`. If everything works, `tx.Commit()`. If anything fails, return early and the deferred Rollback undoes it.

---

## Why this matters for the URL shortener

Scenario: You generate a random code and save. What if two requests generate the same code?

Without a transaction:

```go
// Request 1: generates "abc123", saves
// Request 2: generates "abc123", tries to save — UNIQUE constraint violation
```

With a transaction + retry:

```go
func (s *Service) Shorten(url string) (storage.URL, error) {
    for retries := 0; retries < 3; retries++ {
        code := generateCode(8)
        err := s.repo.Save(storage.URL{Code: code, OriginalURL: url})
        if err == nil {
            return storage.URL{Code: code, OriginalURL: url}, nil
        }
        // If it's a duplicate, retry with a new code
        if strings.Contains(err.Error(), "UNIQUE constraint") {
            continue
        }
        return storage.URL{}, err
    }
    return storage.URL{}, errors.New("failed to generate unique code")
}
```

---

## The `TxRepository` pattern

```go
type TxRepository interface {
    Repository
    WithTx(tx *sql.Tx) TxRepository
}
```

Or, more commonly, pass the `*sql.Tx` to the methods that need it:

```go
type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
    SaveWithTx(tx *sql.Tx, url URL) error
}
```

---

## Senior mindset: "Defer rollback, commit on success"

```go
tx, _ := db.Begin()
defer tx.Rollback() // safe always

// ...do work...

tx.Commit() // if this succeeds, Rollback is a no-op
```

`tx.Rollback()` is safe to call after `tx.Commit()`. So `defer tx.Rollback()` is the Go best practice — it's always there if something fails.

---

## Summary

| Method | Purpose |
|--------|---------|
| `db.Begin()` | Start a transaction |
| `tx.Exec(...)` | Run a query within the transaction |
| `tx.Commit()` | Apply all changes |
| `tx.Rollback()` | Undo all changes |
| `defer tx.Rollback()` | Safety net — no-op if committed |

Open DAY19/DRILL.md.
