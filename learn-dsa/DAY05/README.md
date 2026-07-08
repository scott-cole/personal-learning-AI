# DAY 05 — Prefix Sums

## Running Total on a Receipt

A grocery receipt lists items one by one. At the bottom is a running total that says "if you've bought everything up to this point, you owe this much." If you want to know how much you spent between items 3 and 7, you don't re-add items 3,4,5,6,7 — you just subtract the total at item 2 from the total at item 7.

That's a prefix sum: an array where `prefix[i] = sum(arr[0] + arr[1] + ... + arr[i-1])`.

## Building Prefix Sums

```go
func buildPrefixSum(nums []int) []int {
    prefix := make([]int, len(nums)+1) // extra slot for sum of zero elements
    for i, v := range nums {
        prefix[i+1] = prefix[i] + v
    }
    return prefix
}
```

`prefix[0] = 0` — the sum of zero elements.
`prefix[i]` = sum of elements `nums[0..i-1]`.

## Range Sum Query — O(1) after O(n) prep

```go
// sum of nums[l..r] inclusive
func rangeSum(prefix []int, l, r int) int {
    return prefix[r+1] - prefix[l]
}
```

## 2D Prefix Sum

For matrices, you build a 2D prefix where each cell sums everything above and to the left.

```go
func build2DPrefix(matrix [][]int) [][]int {
    rows, cols := len(matrix), len(matrix[0])
    pref := make([][]int, rows+1)
    for i := range pref {
        pref[i] = make([]int, cols+1)
    }
    for i := 0; i < rows; i++ {
        for j := 0; j < cols; j++ {
            pref[i+1][j+1] = matrix[i][j] + pref[i][j+1] + pref[i+1][j] - pref[i][j]
        }
    }
    return pref
}

// sum of submatrix (r1,c1) to (r2,c2) inclusive
func submatrixSum(pref [][]int, r1, c1, r2, c2 int) int {
    return pref[r2+1][c2+1] - pref[r1][c2+1] - pref[r2+1][c1] + pref[r1][c1]
}
```

## Common Uses

- Range sum queries on static arrays
- Subarray sums (find subarrays summing to k)
- Running balance calculations
- 2D rectangle sum queries

```go
// Count subarrays that sum to k
func subarraySum(nums []int, k int) int {
    count := 0
    sum := 0
    freq := map[int]int{0: 1}
    for _, v := range nums {
        sum += v
        count += freq[sum-k]
        freq[sum]++
    }
    return count
}
```

## Summary

| Concept | Description |
|---|---|
| Prefix sum | `pref[i] = sum of first i elements` |
| Range query | O(1) after O(n) preprocessing |
| 2D prefix | Inclusion-exclusion formula |
| Hash map variant | Count subarrays with given sum |
