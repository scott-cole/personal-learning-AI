# DAY24: Concurrency — "Multiple Chefs in a Kitchen"

---

## The anecdote: "A kitchen with multiple chefs"

One chef can cook one meal at a time. A restaurant needs 20 meals at once. You hire more chefs.

**Chefs = goroutines**
**Counters = channels** (pass ingredients between chefs)
**The fridge = shared memory** (must be locked — mutex)

```
Order 1 → Chef A (cooks pasta) → Plate
Order 2 → Chef B (cooks steak) → Plate
Order 3 → Chef C (cooks salad) → Plate
```

All cooking simultaneously. Each chef works independently. When they need to share something (the fridge), they wait their turn (mutex).

---

## Goroutines

A goroutine is a lightweight thread. Start one with `go`:

```go
func main() {
    go printNumbers() // runs concurrently
    go printLetters() // runs concurrently
    time.Sleep(time.Second) // wait for goroutines
}

func printNumbers() {
    for i := 1; i <= 5; i++ {
        fmt.Println(i)
        time.Sleep(100 * time.Millisecond)
    }
}

func printLetters() {
    for c := 'a'; c <= 'e'; c++ {
        fmt.Println(string(c))
        time.Sleep(100 * time.Millisecond)
    }
}
```

Output (interleaved — order varies):
```
1
a
2
b
c
3
...
```

---

## Mutex — locking shared data

The `MemoryRepository` uses a map. If two goroutines write to a map at the same time, Go **panics** ("concurrent map writes").

Fix: add a mutex.

```go
type MemoryRepository struct {
    mu   sync.Mutex
    data map[string]URL
}

func (r *MemoryRepository) Save(url URL) error {
    r.mu.Lock()
    defer r.mu.Unlock()

    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepository) FindByCode(code string) (URL, error) {
    r.mu.Lock()
    defer r.mu.Unlock()

    url, ok := r.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}
```

`mu.Lock()` — wait until no one else is holding the lock, then grab it.
`defer mu.Unlock()` — release when the function exits.

---

## RWMutex — read vs write locks

`sync.RWMutex` allows multiple concurrent readers but only one writer:

```go
type MemoryRepository struct {
    mu   sync.RWMutex
    data map[string]URL
}

func (r *MemoryRepository) Save(url URL) error {
    r.mu.Lock()        // exclusive — no readers or writers
    defer r.mu.Unlock()
    // ...
}

func (r *MemoryRepository) FindByCode(code string) (URL, error) {
    r.mu.RLock()       // shared — multiple readers allowed
    defer r.mu.RUnlock()
    // ...
}
```

Use `RLock` for reads (many can read simultaneously). Use `Lock` for writes (only one at a time).

---

## Channels

Channels pass data between goroutines:

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "hello from goroutine"
    }()

    msg := <-ch
    fmt.Println(msg) // "hello from goroutine"
}
```

`ch <- value` sends. `<-ch` receives. Both **block** until the other side is ready.

---

## Senior mindset: "Don't communicate by sharing memory; share memory by communicating"

Go proverb. It means: use channels to pass data between goroutines instead of sharing variables with mutexes.

But for the URL shortener, **a mutex is the right tool**. The repository owns the data. Multiple goroutines access it. A mutex protects it.

---

## Summary

| Tool | Purpose | Analogy |
|------|---------|---------|
| `go func()` | Start a goroutine | Hire a new chef |
| `sync.Mutex` | Exclusive access | Key to the fridge |
| `sync.RWMutex` | Read/write locking | Reading the menu (many), writing it (one) |
| `chan` | Pass data between goroutines | Pass an ingredient |

Open DAY24/DRILL.md.
