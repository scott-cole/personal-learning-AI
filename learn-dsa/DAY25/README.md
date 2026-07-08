# DAY 25 — Merge Sort and Quicksort

## Divide and Conquer

A big task is overwhelming. But if you split it into two smaller tasks, give each to a team, and combine the results — suddenly it's manageable. That's divide and conquer.

If each team splits again, you get a tree of teams. Merge sort and quicksort are the two canonical divide-and-conquer sorts. Both are O(n log n) average case.

## Merge Sort

Split the array in half, recursively sort each half, then merge the two sorted halves.

```go
func mergeSort(nums []int) []int {
    if len(nums) <= 1 {
        return nums
    }
    mid := len(nums) / 2
    left := mergeSort(nums[:mid])
    right := mergeSort(nums[mid:])
    return merge(left, right)
}

func merge(left, right []int) []int {
    result := make([]int, 0, len(left)+len(right))
    i, j := 0, 0
    for i < len(left) && j < len(right) {
        if left[i] <= right[j] {
            result = append(result, left[i])
            i++
        } else {
            result = append(result, right[j])
            j++
        }
    }
    result = append(result, left[i:]...)
    result = append(result, right[j:]...)
    return result
}
```

**Properties**: O(n log n) worst case, stable, O(n) space.

## Quicksort

Pick a pivot, partition the array so elements smaller than the pivot are on the left and larger on the right, then recursively sort each partition.

```go
func quickSort(nums []int, lo, hi int) {
    if lo >= hi {
        return
    }
    p := partition(nums, lo, hi)
    quickSort(nums, lo, p-1)
    quickSort(nums, p+1, hi)
}

func partition(nums []int, lo, hi int) int {
    pivot := nums[hi]
    i := lo
    for j := lo; j < hi; j++ {
        if nums[j] < pivot {
            nums[i], nums[j] = nums[j], nums[i]
            i++
        }
    }
    nums[i], nums[hi] = nums[hi], nums[i]
    return i
}
```

**Properties**: O(n log n) average, O(n²) worst (rare with good pivot choice), not stable, O(log n) space (recursion stack).

## Merge Sort vs Quicksort

| Property | Merge Sort | Quicksort |
|---|---|---|
| Time (average) | O(n log n) | O(n log n) |
| Time (worst) | O(n log n) | O(n²) |
| Space | O(n) | O(log n) |
| Stable | Yes | No |
| Cache-friendly | Sequential merge | Partition is linear |
