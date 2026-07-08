# DAY 07 — Project: Array Utilities Package

## Bringing It All Together

This week you learned Big O, arrays/slices, two pointers, sliding windows, prefix sums, and in-place operations. Now you'll build a reusable `arrayutils` Go package with tests. This is how real Go libraries are structured — a package with exported functions and a test file that validates each one.

You'll implement each utility yourself (from scratch), then write table-driven tests.

## Package Structure

```
DAY07/
├── go.mod
├── arrayutils.go      # your implementations
└── arrayutils_test.go  # tests
```

## What to Build

- `Sum(nums []int) int` — O(n)
- `Contains(nums []int, target int) bool` — O(n)
- `Reverse(nums []int)` — in-place, O(1) space
- `Rotate(nums []int, k int)` — in-place, O(1) space
- `MaxSubarraySum(nums []int, k int) int` — sliding window
- `PrefixSum(nums []int) []int` — prefix sum array
- `RangeSum(prefix []int, l, r int) int` — O(1) query
- `IsPalindrome(s string) bool` — two pointers
- `TwoSum(nums []int, target int) (int, int)` — two pointers, assumes sorted

## Writing Tests

```go
func TestSum(t *testing.T) {
    cases := []struct {
        name string
        nums []int
        want int
    }{
        {"empty", []int{}, 0},
        {"single", []int{5}, 5},
        {"multiple", []int{1, 2, 3, 4}, 10},
    }
    for _, tc := range cases {
        t.Run(tc.name, func(t *testing.T) {
            got := Sum(tc.nums)
            if got != tc.want {
                t.Errorf("Sum(%v) = %d; want %d", tc.nums, got, tc.want)
            }
        })
    }
}
```

## Summary

A good utility package:
- Has zero external dependencies
- Has thorough table-driven tests
- Handles edge cases (empty input, nil slices, k=0)
- Uses in-place mutation where appropriate
