# DAY09: Checkpoint

**Closed book.**

1. What is the repository pattern? Why is it useful?
   repo pattern is the bottom layer between the service and the database, dont have to talk directly to the database

2. In Go, what makes a function/variable name "exported" (public) vs "unexported" (private)?
   Capital is public lowercase is private

3. How would you import the `storage` package if your module is `learn-go/DAY09`?
   import (
   "learn-go/DAY09/storage"
   )

4. Why would you use an interface for `Repository` instead of just using `MemoryRepository` directly everywhere?
   but the main reason is swappability: you can swap MemoryRepository for SQLRepository (or a test mock) without changing any code that uses the interface. Also lets you have one Save/FindByCode contract enforced at compile time.r import cycles

5. The `MemoryRepository` uses `map[string]URL`. Why `URL` (value) and not `*URL` (pointer)?

Using URL (value) means the map stores a copy — if you get a URL out, modify it, and re-save, you've only changed the local copy, not what's in the map. Using \*URL (pointer) would mean multiple parts of the code share the same URL object, risking accidental mutation. Value types in maps are safer because they're immutable once stored — you must explicitly Save to update.
