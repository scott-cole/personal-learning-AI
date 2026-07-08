# DAY 01 — Big O Notation

## The Queue at the Coffee Shop

Imagine you walk into a busy coffee shop. There's a single counter and one barista. If one person is ahead of you, you wait a little. If ten people are ahead, you wait longer. If the barista suddenly grows three extra arms (more workers), the line shrinks. This is exactly what Big O measures: **how much longer your wait gets as more people arrive**.

- O(1) — The barista hands you a pre-made drink. Instant. No matter how many people.
- O(n) — The barista serves each person one by one. Twice the people = twice the wait.
- O(n²) — The barista asks every person to shake hands with every other person before ordering. A nightmare.
- O(log n) — The barista yells "Anyone named Alex?" and only Alex steps forward. You halve the search every time.
- O(2ⁿ) — The barista calls every possible subset of people to check if anyone wants a latte. The shop burns down.

Big O describes the **worst-case** growth rate of time or memory as input size `n` grows. We drop constants and lower-order terms because we care about the shape of the curve, not the exact seconds.

## Time Complexity

```go
// O(1) — Constant time
func getFirst(nums []int) int {
    return nums[0]
}

// O(n) — Linear time
func sum(nums []int) int {
    total := 0
    for _, v := range nums {
        total += v
    }
    return total
}

// O(n²) — Quadratic time
func pairs(nums []int) {
    for i := 0; i < len(nums); i++ {
        for j := 0; j < len(nums); j++ {
            fmt.Println(nums[i], nums[j])
        }
    }
}

// O(log n) — Logarithmic time
func binarySearch(nums []int, target int) int {
    lo, hi := 0, len(nums)-1
    for lo <= hi {
        mid := (lo + hi) / 2
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

## Space Complexity

Space complexity measures how much extra memory your algorithm needs.

```go
// O(1) space — just a few variables
func minMax(nums []int) (int, int) {
    min, max := nums[0], nums[0]
    for _, v := range nums {
        if v < min {
            min = v
        }
        if v > max {
            max = v
        }
    }
    return min, max
}

// O(n) space — creates a copy
func double(nums []int) []int {
    result := make([]int, len(nums))
    for i, v := range nums {
        result[i] = v * 2
    }
    return result
}
```

## Common Runtimes

| Notation | Name | Example |
|---|---|---|
| O(1) | Constant | Array access, hash lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Iterating a slice |
| O(n log n) | Linearithmic | Sorting (merge sort, quick sort) |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Recursive Fibonacci (naive) |
| O(n!) | Factorial | Generating all permutations |

## Summary

- Big O describes **growth rate** — how runtime/memory scales with input size.
- Always consider **worst case** unless otherwise stated.
- Drop constants: O(2n) → O(n), O(n² + n) → O(n²).
- Space complexity matters too.
- Measure, don't guess — but Big O gives you a mental model before you write code.
