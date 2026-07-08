# DAY25: Checkpoint

**Closed book.**

1. What does `signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)` do?

2. What does `srv.Shutdown(ctx)` do that `os.Exit(0)` doesn't?

3. What happens if `Shutdown` takes longer than the context timeout?

4. Why do we start `srv.ListenAndServe()` in a goroutine?

5. What's the difference between SIGINT and SIGTERM?
