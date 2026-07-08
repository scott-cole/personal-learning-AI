# DRILL — Simple Sorts

## Task

Implement bubble sort, selection sort, and insertion sort. Benchmark each.

## Starter Code

Create `main.go`:

```go
package main

import (
    "fmt"
    "time"
)

// TODO: Implement bubble sort in-place.
func BubbleSort(nums []int) {
}

// TODO: Implement selection sort in-place.
func SelectionSort(nums []int) {
}

// TODO: Implement insertion sort in-place.
func InsertionSort(nums []int) {
}

func main() {
    data := []int{5, 2, 8, 1, 9, 3, 7, 4, 6}

    // Test each sort on a copy
    for name, sortFn := range map[string]func([]int){
        "Bubble":    BubbleSort,
        "Selection": SelectionSort,
        "Insertion": InsertionSort,
    } {
        arr := make([]int, len(data))
        copy(arr, data)
        start := time.Now()
        sortFn(arr)
        fmt.Printf("%s: %v (%v)\n", name, arr, time.Since(start))
    }
}
```

## Expected Output

```
Bubble: [1 2 3 4 5 6 7 8 9] (...)
Selection: [1 2 3 4 5 6 7 8 9] (...)
Insertion: [1 2 3 4 5 6 7 8 9] (...)
```

Timing will vary — each should be microseconds for 9 elements.

## Hints

- Bubble: outer loop n-1 passes, inner loop compares adjacent.
- Selection: outer loop picks position, inner finds minimum from i to end.
- Insertion: start from index 1, shift elements right until finding the insertion point.

## Run

```bash
go run main.go
```
