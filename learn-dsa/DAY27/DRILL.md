# DRILL — Dynamic Programming

## Task

Implement Fibonacci with both memoization and tabulation, then solve 0/1 knapsack.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Return nth Fibonacci number using memoization.
func FibMemo(n int) int {
    return 0
}

// TODO: Return nth Fibonacci number using tabulation.
func FibTab(n int) int {
    return 0
}

// TODO: Return the max value for 0/1 knapsack.
func Knapsack(weights, values []int, capacity int) int {
    return 0
}

func main() {
    fmt.Println("FibMemo(10):", FibMemo(10))
    fmt.Println("FibTab(10):", FibTab(10))
    fmt.Println("FibMemo(50):", FibMemo(50))

    weights := []int{2, 3, 4, 5}
    values := []int{3, 4, 5, 6}
    capacity := 5
    fmt.Println("Knapsack(max value):", Knapsack(weights, values, capacity))
}
```

## Expected Output

```
FibMemo(10): 55
FibTab(10): 55
FibMemo(50): 12586269025
Knapsack(max value): 7
```

## Hints

- Memoization: create a cache slice, check before computing, store after.
- Tabulation: build a dp slice iteratively.
- Knapsack: 2D DP table — rows are items, columns are capacities.

## Run

```bash
go run main.go
```
