# DAY 24 — Simple Sorts

## Organising Cards by Hand

You're dealt a hand of 7 cards. You sort them:
- **Bubble sort**: scan left to right, swap adjacent out-of-order pairs. Repeat until no swaps. Slow but simple.
- **Selection sort**: find the smallest card, put it first. Find the next smallest, put it second. Repeat.
- **Insertion sort**: take one card at a time and insert it into the correct position in your already-sorted left hand.

These are O(n²) sorts — fine for small datasets but terrible for large ones. They're worth knowing as building blocks and as examples of different algorithmic paradigms.

## Bubble Sort

```go
func bubbleSort(nums []int) {
    n := len(nums)
    for i := 0; i < n-1; i++ {
        swapped := false
        for j := 0; j < n-i-1; j++ {
            if nums[j] > nums[j+1] {
                nums[j], nums[j+1] = nums[j+1], nums[j]
                swapped = true
            }
        }
        if !swapped {
            break // already sorted
        }
    }
}
```

## Selection Sort

```go
func selectionSort(nums []int) {
    n := len(nums)
    for i := 0; i < n-1; i++ {
        minIdx := i
        for j := i + 1; j < n; j++ {
            if nums[j] < nums[minIdx] {
                minIdx = j
            }
        }
        nums[i], nums[minIdx] = nums[minIdx], nums[i]
    }
}
```

## Insertion Sort

```go
func insertionSort(nums []int) {
    for i := 1; i < len(nums); i++ {
        key := nums[i]
        j := i - 1
        for j >= 0 && nums[j] > key {
            nums[j+1] = nums[j]
            j--
        }
        nums[j+1] = key
    }
}
```

## Comparison

| Sort | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |

Insertion sort is the most practical of the three — it's excellent for small arrays and nearly-sorted data (used as the base case in Timsort, Python's and Go's sort).
