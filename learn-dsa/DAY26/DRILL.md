# DRILL — Recursion

## Task

Implement factorial, generate subsets, and generate permutations using recursion.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Return n! (factorial).
func Factorial(n int) int {
    return 0
}

// TODO: Return all subsets of nums.
func Subsets(nums []int) [][]int {
    return nil
}

// TODO: Return all permutations of nums.
func Permute(nums []int) [][]int {
    return nil
}

func main() {
    fmt.Println("Factorial 5:", Factorial(5))
    fmt.Println("Factorial 0:", Factorial(0))

    fmt.Println("Subsets of [1,2]:")
    for _, s := range Subsets([]int{1, 2}) {
        fmt.Println(" ", s)
    }

    fmt.Println("Permutations of [1,2,3]:")
    for _, p := range Permute([]int{1, 2, 3}) {
        fmt.Println(" ", p)
    }
}
```

## Expected Output

```
Factorial 5: 120
Factorial 0: 1
Subsets of [1,2]:
  []
  [1]
  [2]
  [1 2]
Permutations of [1,2,3]:
  [1 2 3]
  [1 3 2]
  [2 1 3]
  [2 3 1]
  [3 1 2]
  [3 2 1]
```

## Hints

- Factorial: `n * Factorial(n-1)` with base case n ≤ 1.
- Subsets: at each index, you can either include or exclude the element.
- Permutations: keep track of used elements with a `used[]` bool slice.

## Run

```bash
go run main.go
```
