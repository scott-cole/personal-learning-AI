# DRILL — Prefix Sums

## Task

Implement a prefix sum array and use it for range queries.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Build the prefix sum array (extra slot at index 0).
func PrefixSum(nums []int) []int {
    return nil
}

// TODO: Return the sum of nums[l..r] inclusive using the prefix array.
func RangeSum(prefix []int, l, r int) int {
    return 0
}

// TODO: Return the total number of subarrays that sum to k.
func SubarraySum(nums []int, k int) int {
    return 0
}

func main() {
    nums := []int{1, 2, 3, 4, 5, 6}
    pref := PrefixSum(nums)

    fmt.Println("nums:", nums)
    fmt.Println("prefix:", pref)
    fmt.Println("sum[2..4]:", RangeSum(pref, 2, 4)) // 3+4+5=12
    fmt.Println("sum[0..5]:", RangeSum(pref, 0, 5)) // 21
    fmt.Println("sum[3..3]:", RangeSum(pref, 3, 3)) // 4

    nums2 := []int{1, 1, 1}
    fmt.Println("subarrays summing to 2:", SubarraySum(nums2, 2)) // 2
}
```

## Expected Output

```
nums: [1 2 3 4 5 6]
prefix: [0 1 3 6 10 15 21]
sum[2..4]: 12
sum[0..5]: 21
sum[3..3]: 4
subarrays summing to 2: 2
```

## Hints

- PrefixSum should return `len(nums)+1` elements.
- RangeSum: `prefix[r+1] - prefix[l]`.
- SubarraySum: use a map to store frequency of prefix sums seen.

## Run

```bash
go run main.go
```
