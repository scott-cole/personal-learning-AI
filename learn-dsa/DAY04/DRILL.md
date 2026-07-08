# DRILL — Sliding Window

## Task

Implement two sliding window functions.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Return the maximum sum of any contiguous subarray of length k.
func MaxSumSubarray(nums []int, k int) int {
    return 0
}

// TODO: Return the length of the longest substring without repeating characters.
func LongestSubstring(s string) int {
    return 0
}

func main() {
    nums := []int{2, 1, 5, 1, 3, 2}
    k := 3
    fmt.Printf("Max sum of subarray size %d: %d\n", k, MaxSumSubarray(nums, k))

    tests := []string{"abcabcbb", "bbbbb", "pwwkew", ""}
    for _, t := range tests {
        fmt.Printf("Longest substring in '%s': %d\n", t, LongestSubstring(t))
    }
}
```

## Expected Output

```
Max sum of subarray size 3: 9
Longest substring in 'abcabcbb': 3
Longest substring in 'bbbbb': 1
Longest substring in 'pwwkew': 3
Longest substring in '': 0
```

## Hints

- For MaxSumSubarray: sum the first k elements, then slide.
- For LongestSubstring: use a map to track character positions.

## Run

```bash
go run main.go
```
