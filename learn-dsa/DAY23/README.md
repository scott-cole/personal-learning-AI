# DAY 23 — Binary Search

## Finding a Word in a Dictionary

How do you find the word "binary" in a dictionary? You don't start at A and flip every page. You open roughly in the middle. "Binary" comes before "computer" — so you go left. You split the left half. "Algorithm" comes before "binary" — so you go right. In 3-4 splits, you're at the right page.

That's binary search: repeatedly divide the search space in half. O(log n) — exponentially faster than linear search for large inputs.

## Iterative Binary Search

```go
func binarySearch(nums []int, target int) int {
    lo, hi := 0, len(nums)-1
    for lo <= hi {
        mid := lo + (hi-lo)/2 // avoid overflow
        if nums[mid] == target {
            return mid
        }
        if nums[mid] < target {
            lo = mid + 1
        } else {
            hi = mid - 1
        }
    }
    return -1
}
```

## Recursive Binary Search

```go
func binarySearchRec(nums []int, target int) int {
    var search func(lo, hi int) int
    search = func(lo, hi int) int {
        if lo > hi {
            return -1
        }
        mid := lo + (hi-lo)/2
        if nums[mid] == target {
            return mid
        }
        if nums[mid] < target {
            return search(mid+1, hi)
        }
        return search(lo, mid-1)
    }
    return search(0, len(nums)-1)
}
```

## Search in Rotated Sorted Array

```go
func searchRotated(nums []int, target int) int {
    lo, hi := 0, len(nums)-1
    for lo <= hi {
        mid := lo + (hi-lo)/2
        if nums[mid] == target {
            return mid
        }
        // Left half is sorted
        if nums[lo] <= nums[mid] {
            if target >= nums[lo] && target < nums[mid] {
                hi = mid - 1
            } else {
                lo = mid + 1
            }
        } else { // Right half is sorted
            if target > nums[mid] && target <= nums[hi] {
                lo = mid + 1
            } else {
                hi = mid - 1
            }
        }
    }
    return -1
}
```

## First and Last Position

```go
func firstPosition(nums []int, target int) int {
    lo, hi := 0, len(nums)-1
    result := -1
    for lo <= hi {
        mid := lo + (hi-lo)/2
        if nums[mid] == target {
            result = mid
            hi = mid - 1 // keep searching left
        } else if nums[mid] < target {
            lo = mid + 1
        } else {
            hi = mid - 1
        }
    }
    return result
}
```

## Summary

| Variant | Condition | Return |
|---|---|---|
| Standard | Exact match | Index or -1 |
| Lower bound | First ≥ target | Index |
| Upper bound | Last ≤ target | Index |
| Rotated | Match in rotated array | Index or -1 |
