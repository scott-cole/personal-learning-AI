# DAY28: Final Checkpoint

**This is cumulative. Answer without looking back.**

1. Write a `Repository` interface with `Save`, `FindByCode`, `IncrementAccess`, and `List` methods.

2. Write a `Service` struct that takes a `Repository` and has `Shorten`, `Resolve`, and `Stats` methods.

3. Write a logging middleware.

4. Write a rate limiting middleware (token bucket).

5. Write a handler that reads JSON from a POST body, calls the service, and returns JSON.

6. Write a migration file that creates a `visits` table.

7. Write the Go code for graceful shutdown.

8. Why do we use `internal/` packages?

9. What's the difference between `sync.Mutex` and `sync.RWMutex`?

10. How do you test a service that depends on a repository?
