# DAY 04 — Sliding Window

## A Magnifying Glass Moving Across a Map

Imagine you're searching a treasure map with a magnifying glass. The glass covers a small area — say 3x3 inches. You slide it left to right, top to bottom, never lifting it off the paper. Whatever is under the glass at any moment is your "window." When you slide it one inch right, you lose a column on the left and gain a column on the right.

That's the sliding window technique: maintain a contiguous subarray (the window) that grows and shrinks as you move a left and right pointer. You update the result based on the window's contents.

## Fixed Window: Max Sum Subarray of Size K

```go
func maxSumSubarray(nums []int, k int) int {
    if len(nums) < k {
        return 0
    }
    // Sum of first window
    windowSum := 0
    for i := 0; i < k; i++ {
        windowSum += nums[i]
    }
    maxSum := windowSum

    // Slide the window
    for i := k; i < len(nums); i++ {
        windowSum += nums[i] - nums[i-k]
        if windowSum > maxSum {
            maxSum = windowSum
        }
    }
    return maxSum
}
```

The window is [i-k+1 ... i]. We subtract the element leaving the window and add the one entering — O(n) instead of O(n·k).

## Dynamic Window: Longest Substring Without Repeating Characters

```go
func lengthOfLongestSubstring(s string) int {
    seen := make(map[byte]int) // char -> last index+1
    left, maxLen := 0, 0

    for right := 0; right < len(s); right++ {
        c := s[right]
        if last, ok := seen[c]; ok && last >= left {
            left = last // move left past the duplicate
        }
        seen[c] = right + 1
        if right-left+1 > maxLen {
            maxLen = right - left + 1
        }
    }
    return maxLen
}
```

The window shrinks when we encounter a duplicate — the left pointer jumps ahead.

## When to Use Sliding Window

- The problem asks for "subarray" or "substring."
- There's a constraint on the window (size, unique characters, sum).
- Brute force would be O(n²) or worse.

## Common Patterns

| Type | Window Size | Example |
|---|---|---|
| Fixed | Constant K | Max sum of K elements |
| Dynamic | Variable | Longest substring without repeats |
| Dynamic with auxiliary | Variable | Min window containing all characters |

## Summary

- Sliding window reduces O(n·k) to O(n).
- Fixed window: compute first window, then slide by subtracting left + adding right.
- Dynamic window: expand right, shrink left when constraint is violated.
