# DRILL — Trade-Offs Analysis

## Task

Benchmark array vs linked list for different operations. Draw conclusions.

## Starter Code

Create `main.go`:

```go
package main

import (
    "container/list"
    "fmt"
    "time"
)

// TODO: Measure the time to insert at index 0 n times using a slice.
func benchmarkSlicePrepend(n int) time.Duration {
    var s []int
    start := time.Now()
    for i := 0; i < n; i++ {
        // TODO: insert i at the beginning of s
        _ = i
    }
    return time.Since(start)
}

// TODO: Measure the time to prepend n times using container/list.
func benchmarkListPrepend(n int) time.Duration {
    l := list.New()
    start := time.Now()
    for i := 0; i < n; i++ {
        // TODO: insert i at the front
        _ = i
    }
    return time.Since(start)
}

// TODO: Measure the time to append n times using a slice.
func benchmarkSliceAppend(n int) time.Duration {
    var s []int
    start := time.Now()
    for i := 0; i < n; i++ {
        s = append(s, i)
    }
    return time.Since(start)
}

// TODO: Measure the time to iterate over n elements.
func benchmarkSliceIterate(n int) time.Duration {
    s := make([]int, n)
    start := time.Now()
    for i := 0; i < len(s); i++ {
        _ = s[i]
    }
    return time.Since(start)
}

func main() {
    n := 100000
    fmt.Println("Benchmarking with n =", n)
    fmt.Println()

    fmt.Println("Slice prepend:", benchmarkSlicePrepend(n))
    fmt.Println("List prepend:", benchmarkListPrepend(n))
    fmt.Println()
    fmt.Println("Slice append:", benchmarkSliceAppend(n))
    fmt.Println()

    // Build a list first, then iterate
    l := list.New()
    for i := 0; i < n; i++ {
        l.PushFront(i)
    }
    start := time.Now()
    for e := l.Front(); e != nil; e = e.Next() {
        _ = e.Value
    }
    fmt.Println("List iterate:", time.Since(start))

    fmt.Println("Slice iterate:", benchmarkSliceIterate(n))
}
```

## Expected Behaviour

```
Benchmarking with n = 100000

Slice prepend: ~5s (very slow — O(n²))
List prepend: ~10ms (very fast — O(1) each)
Slice append: ~2ms (fast — O(1) amortised)
List iterate: ~15ms
Slice iterate: ~500µs
```

Your times may vary, but the **relative** differences should be dramatic.

## Hints

- For slice prepend: `s = append([]int{i}, s...)` or use insert logic.
- For list prepend: `l.PushFront(i)`.
- Run with `go run main.go` and observe the magnitude differences.

## Run

```bash
go run main.go
```
