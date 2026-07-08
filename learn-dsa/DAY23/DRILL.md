# DRILL — Binary Search

## Task

Implement standard binary search, search in rotated array, and first/last position.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Return index of target in sorted nums, or -1.
func BinarySearch(nums []int, target int) int {
    return -1
}

// TODO: Return index of target in rotated sorted array, or -1.
// Example: [4,5,6,7,0,1,2] target=0 → 4
func SearchRotated(nums []int, target int) int {
    return -1
}

// TODO: Return [first, last] index of target in sorted nums.
// Example: [1,2,2,2,3] target=2 → [1,3]
func SearchRange(nums []int, target int) []int {
    return []int{-1, -1}
}

func main() {
    nums := []int{1, 3, 5, 7, 9, 11, 13}
    fmt.Println("BinarySearch 7:", BinarySearch(nums, 7))
    fmt.Println("BinarySearch 8:", BinarySearch(nums, 8))

    rotated := []int{4, 5, 6, 7, 0, 1, 2}
    fmt.Println("SearchRotated 0:", SearchRotated(rotated, 0))
    fmt.Println("SearchRotated 3:", SearchRotated(rotated, 3))

    dupes := []int{1, 2, 2, 2, 3, 4, 5}
    fmt.Println("SearchRange 2:", SearchRange(dupes, 2))
    fmt.Println("SearchRange 6:", SearchRange(dupes, 6))
}
```

## Expected Output

```
BinarySearch 7: 3
BinarySearch 8: -1
SearchRotated 0: 4
SearchRotated 3: -1
SearchRange 2: [1 3]
SearchRange 6: [-1 -1]
```

## Hints

- Standard binary search: `mid = lo + (hi-lo)/2`.
- Rotated: determine which half is sorted, then decide which half to search.
- Range: use separate binary searches for first and last positions.

## Run

```bash
go run main.go
```
