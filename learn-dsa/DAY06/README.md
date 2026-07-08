# DAY 06 — In-Place Operations

## Rearranging Furniture Without a Moving Truck

When you want to rearrange your living room, you could rent a truck, move everything to a storage unit, sort it out, and bring it back. That's the O(n) space approach. Or you could just shuffle things around in the room — push the sofa to the wall, slide the coffee table, swap the lamp with the plant. That's in-place: O(1) extra space, just moving what's already there.

In-place algorithms transform data using only a constant amount of extra memory. The input is overwritten.

## Reversing an Array In-Place

```go
func reverse(nums []int) {
    for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
        nums[i], nums[j] = nums[j], nums[i]
    }
}
```

## Rotating an Array by K Positions

```go
// Reverse segments to rotate in-place
func rotate(nums []int, k int) {
    n := len(nums)
    k %= n
    reverse(nums)       // full reverse
    reverse(nums[:k])   // reverse first k
    reverse(nums[k:])   // reverse the rest
}
```

This triple-reverse trick is O(n) time, O(1) space.

## Partitioning (Dutch National Flag)

Sort an array of 0s, 1s, and 2s in-place.

```go
func sortColors(nums []int) {
    low, mid, high := 0, 0, len(nums)-1
    for mid <= high {
        switch nums[mid] {
        case 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low++
            mid++
        case 1:
            mid++
        case 2:
            nums[mid], nums[high] = nums[high], nums[mid]
            high--
        }
    }
}
```

## Removing Elements In-Place

```go
// Remove all occurrences of val, return new length
func removeElement(nums []int, val int) int {
    k := 0
    for _, v := range nums {
        if v != val {
            nums[k] = v
            k++
        }
    }
    return k
}
```

## When In-Place Matters

- Memory-constrained environments (embedded systems)
- Large data that doesn't fit in RAM twice
- APIs where you're expected to mutate the input
- LeetCode-style problems that explicitly ask for O(1) space

## Summary

| Operation | In-Place Strategy | Time |
|---|---|---|
| Reverse | Swap ends moving inward | O(n) |
| Rotate | Triple reverse | O(n) |
| Partition | Three pointers (low, mid, high) | O(n) |
| Remove | Slow/fast write pointer | O(n) |
