# DRILL — Big O Classification

## Task

Implement a function for each scenario, then determine its Big O complexity.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Implement this function
// FindMax returns the maximum value in a slice.
func FindMax(nums []int) int {
    // your code here
    return 0
}

// TODO: Implement this function
// ContainsDuplicate returns true if any value appears twice.
func ContainsDuplicate(nums []int) bool {
    // your code here
    return false
}

// TODO: Implement this function
// PrintAllPairs prints every pair (i, j) from nums.
func PrintAllPairs(nums []int) {
    // your code here
}

func main() {
    nums := []int{3, 7, 1, 9, 4, 7, 2}

    fmt.Println("Max:", FindMax(nums))
    fmt.Println("Has duplicate:", ContainsDuplicate(nums))
    fmt.Println("All pairs:")
    PrintAllPairs(nums)
}
```

## Expected Output

```
Max: 9
Has duplicate: true
All pairs:
(3,3) (3,7) ... (2,2)
```

## Hints

- For `ContainsDuplicate`, use a map as a set.
- `PrintAllPairs` uses two nested loops.
- After implementing, write a comment above each function stating its Big O.

## Run

```bash
go run main.go
```
