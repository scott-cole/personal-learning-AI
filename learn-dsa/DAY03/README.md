# DAY 03 — Two-Pointer Technique

## Two Fingers Bookmarking a Page

When reading a dense textbook, you might put one finger on the key definition and another on the diagram that explains it. You move them independently to compare parts of the text. That's exactly the two-pointer technique: placing two indices (fingers) in your data and moving them strategically.

Two pointers solve problems that would otherwise need nested loops. Instead of O(n²), you get O(n).

## Palindrome Check

A palindrome reads the same forwards and backwards — like "racecar".

```go
func isPalindrome(s string) bool {
    left, right := 0, len(s)-1
    for left < right {
        if s[left] != s[right] {
            return false
        }
        left++
        right--
    }
    return true
}
```

Left and right pointers start at opposite ends and move toward each other. If they ever disagree, it's not a palindrome.

## Pair Sum (Sorted Array)

Find two numbers in a sorted slice that sum to a target.

```go
func pairSum(nums []int, target int) (int, int) {
    left, right := 0, len(nums)-1
    for left < right {
        sum := nums[left] + nums[right]
        if sum == target {
            return left, right
        }
        if sum < target {
            left++ // need a bigger number
        } else {
            right-- // need a smaller number
        }
    }
    return -1, -1
}
```

## Reversing a Slice

```go
func reverse(nums []int) {
    left, right := 0, len(nums)-1
    for left < right {
        nums[left], nums[right] = nums[right], nums[left]
        left++
        right--
    }
}
```

## Remove Duplicates from Sorted Array

One pointer (`slow`) tracks where to write, another (`fast`) scans ahead.

```go
func removeDuplicates(nums []int) int {
    if len(nums) == 0 {
        return 0
    }
    slow := 0
    for fast := 1; fast < len(nums); fast++ {
        if nums[fast] != nums[slow] {
            slow++
            nums[slow] = nums[fast]
        }
    }
    return slow + 1
}
```

## Common Patterns

| Pattern | Moves | Typical Use |
|---|---|---|
| Opposite ends | L→, R← | Palindrome, pair sum |
| Same direction | Slow, Fast | Remove duplicates, cycle detection |
| Partition | L→, R→ | Quicksort, Dutch flag |

## Summary

- Two pointers replace nested loops with a single pass.
- Great for sorted arrays, linked lists, and palindrome/string problems.
- Always ask: can I solve this by tracking two positions?
