# DRILL — Merge Sort and Quicksort

## Task

Implement both merge sort and quicksort.

## Starter Code

Create `main.go`:

```go
package main

import (
    "fmt"
    "reflect"
)

// TODO: Implement merge sort (return a new sorted slice).
func MergeSort(nums []int) []int {
    return nil
}

// TODO: Implement quicksort (in-place).
func QuickSort(nums []int) {
}

func main() {
    data1 := []int{38, 27, 43, 3, 9, 82, 10}
    sorted1 := MergeSort(data1)
    fmt.Println("MergeSort:", sorted1)
    fmt.Println("Original preserved:", reflect.DeepEqual(data1, []int{38, 27, 43, 3, 9, 82, 10}))

    data2 := []int{38, 27, 43, 3, 9, 82, 10}
    QuickSort(data2)
    fmt.Println("QuickSort:", data2)
}
```

## Expected Output

```
MergeSort: [3 9 10 27 38 43 82]
Original preserved: true
QuickSort: [3 9 10 27 38 43 82]
```

## Hints

- Merge sort: recursively split, merge with two pointers.
- Quick sort: partition using last element as pivot, recursively sort left and right.
- For merge sort, the `merge` function needs a temporary slice.

## Run

```bash
go run main.go
```
