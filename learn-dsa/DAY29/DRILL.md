# DRILL — Greedy Algorithms

## Task

Implement interval scheduling (max non-overlapping intervals).

## Starter Code

Create `main.go`:

```go
package main

import (
    "fmt"
    "sort"
)

type Interval struct {
    Start, End int
}

// TODO: Select the maximum number of non-overlapping intervals.
// Sort by end time, then greedily pick compatible intervals.
func IntervalScheduling(intervals []Interval) []Interval {
    return nil
}

// TODO: Given coin denominations sorted descending, make change for amount.
// Return a map of coin -> count. Assume greedy works for the given coin set.
func CoinChangeGreedy(coins []int, amount int) map[int]int {
    return nil
}

func main() {
    intervals := []Interval{
        {1, 3}, {2, 5}, {3, 7}, {1, 2}, {5, 8}, {6, 9},
    }
    selected := IntervalScheduling(intervals)
    fmt.Println("Selected intervals:")
    for _, inv := range selected {
        fmt.Printf("  [%d, %d)\n", inv.Start, inv.End)
    }
    fmt.Printf("Count: %d\n", len(selected))

    coins := []int{25, 10, 5, 1}
    amount := 67
    change := CoinChangeGreedy(coins, amount)
    fmt.Printf("Change for %d cents:\n", amount)
    for coin, count := range change {
        if count > 0 {
            fmt.Printf("  %d x %d-cent\n", count, coin)
        }
    }
}
```

## Expected Output

```
Selected intervals:
  [1, 2)
  [2, 5)
  [5, 8)
Count: 3
Change for 67 cents:
  2 x 25-cent
  1 x 10-cent
  1 x 5-cent
  2 x 1-cent
```

## Hints

- Interval scheduling: sort by `.End`, then iterate picking any interval whose start ≥ last end.
- Coin change: iterate coins from largest to smallest, count = amount / coin, subtract.

## Run

```go run main.go```
