# DAY 26 — Recursion Fundamentals

## Matryoshka Dolls

A matryoshka doll opens to reveal a smaller doll inside, which opens to reveal an even smaller doll, until you reach the smallest solid doll. That's recursion: a function that contains a smaller version of itself.

Every recursive function needs:
1. **Base case** — the smallest doll that doesn't open
2. **Recursive case** — the current doll containing a smaller one

## Factorial

```go
func factorial(n int) int {
    if n <= 1 {
        return 1 // base case
    }
    return n * factorial(n-1) // recursive case
}
```

## Fibonacci

```go
func fibonacci(n int) int {
    if n <= 1 {
        return n
    }
    return fibonacci(n-1) + fibonacci(n-2)
}
```

**Warning**: naive Fibonacci is O(2ⁿ). Memoization or iteration is vastly better.

## Permutations

```go
func permute(nums []int) [][]int {
    var result [][]int
    var backtrack func(path []int, used []bool)
    backtrack = func(path []int, used []bool) {
        if len(path) == len(nums) {
            perm := make([]int, len(path))
            copy(perm, path)
            result = append(result, perm)
            return
        }
        for i := 0; i < len(nums); i++ {
            if used[i] {
                continue
            }
            used[i] = true
            path = append(path, nums[i])
            backtrack(path, used)
            path = path[:len(path)-1]
            used[i] = false
        }
    }
    backtrack([]int{}, make([]bool, len(nums)))
    return result
}
```

## Subsets (Power Set)

```go
func subsets(nums []int) [][]int {
    var result [][]int
    var backtrack func(start int, path []int)
    backtrack = func(start int, path []int) {
        subset := make([]int, len(path))
        copy(subset, path)
        result = append(result, subset)

        for i := start; i < len(nums); i++ {
            path = append(path, nums[i])
            backtrack(i+1, path)
            path = path[:len(path)-1]
        }
    }
    backtrack(0, []int{})
    return result
}
```

## When to Use Recursion

- Problems that can be broken into smaller subproblems (trees, divide-and-conquer)
- Problems with branching choices (permutations, subsets, N-queens)
- Recursive data structures (trees, linked lists)

## Summary

| Problem | Base Case | Recurrence |
|---|---|---|
| Factorial | n ≤ 1 | n × f(n-1) |
| Fibonacci | n ≤ 1 | f(n-1) + f(n-2) |
| Permutations | path length = n | for each unused, add and recurse |
| Subsets | start = n | for each i, include nums[i] and recurse |
