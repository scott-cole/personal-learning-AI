# DRILL — Array Utilities Package

## Task

Build a complete Go package with array utility functions and tests. Run `go test` to verify everything.

## Starter Code

### go.mod

```
module github.com/learn-dsa/arrayutils

go 1.21
```

### arrayutils.go

```go
package arrayutils

// TODO: Return the sum of all elements.
func Sum(nums []int) int {
    return 0
}

// TODO: Return true if target is in nums.
func Contains(nums []int, target int) bool {
    return false
}

// TODO: Reverse nums in-place.
func Reverse(nums []int) {
}

// TODO: Rotate nums right by k in-place.
func Rotate(nums []int, k int) {
}

// TODO: Return max sum of any contiguous subarray of length k.
func MaxSubarraySum(nums []int, k int) int {
    return 0
}

// TODO: Build prefix sum array.
func PrefixSum(nums []int) []int {
    return nil
}

// TODO: Return sum of nums[l..r] inclusive using prefix array.
func RangeSum(prefix []int, l, r int) int {
    return 0
}

// TODO: Return true if s is a palindrome (two pointers).
func IsPalindrome(s string) bool {
    return false
}

// TODO: Return indices of two numbers that sum to target (sorted input).
func TwoSum(nums []int, target int) (int, int) {
    return -1, -1
}
```

### arrayutils_test.go

Write table-driven tests for each function. Cover edge cases:
- Empty slices
- Single elements
- Negative numbers
- k=0 for Rotate

## Expected Test Output

```
$ go test -v
=== RUN   TestSum
=== RUN   TestSum/empty
=== RUN   TestSum/single
=== RUN   TestSum/multiple
--- PASS: TestSum (0.00s)
...
```

## Hints

- Run `go mod init` then `go test`.
- Use `t.Run` for subtests.
- Test `Rotate` with k larger than the array.

## Run

```bash
go mod init github.com/learn-dsa/arrayutils
go test -v
```
