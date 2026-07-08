# DAY20: Checkpoint

**Closed book.**

1. What does `%w` do in `fmt.Errorf`? How is it different from `%v`?

2. What does `errors.Is(err, sql.ErrNoRows)` check?

3. At which layer(s) should you log errors? Which layer should you NOT log at?

4. Why should you wrap errors at layer boundaries specifically?

5. What's the advantage of `fmt.Errorf("save %q: %w", url.Code, err)` over just returning `err`?
