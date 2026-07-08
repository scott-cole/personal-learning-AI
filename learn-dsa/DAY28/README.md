# DAY 28 — Backtracking

## Trying Keys on a Key Ring

You have a key ring with 5 similar keys. You need to unlock a door. You try the first key — doesn't fit. You try the second — doesn't fit. The third fits but won't turn all the way. You backtrack: take it out, try the next one. Eventually you find the right key, or you exhaust all options.

Backtracking is a systematic trial-and-error approach: make a choice, recurse, undo the choice (backtrack), and try the next option. It's used for constraint satisfaction problems: permutations, subsets, N-queens, Sudoku.

## Template

```go
func backtrack(candidate, path, result) {
    if candidate is valid {
        add to result
    }
    for each option {
        make choice
        backtrack(next candidate, path, result)
        undo choice
    }
}
```

## N-Queens

Place N queens on an N×N board so no two attack each other.

```go
func solveNQueens(n int) [][]string {
    var result [][]string
    board := make([][]rune, n)
    for i := range board {
        board[i] = []rune(strings.Repeat(".", n))
    }
    cols := make([]bool, n)
    diag1 := make([]bool, 2*n-1) // r + c
    diag2 := make([]bool, 2*n-1) // r - c + n - 1

    var backtrack func(row int)
    backtrack = func(row int) {
        if row == n {
            solution := make([]string, n)
            for i, line := range board {
                solution[i] = string(line)
            }
            result = append(result, solution)
            return
        }
        for col := 0; col < n; col++ {
            d1 := row + col
            d2 := row - col + n - 1
            if cols[col] || diag1[d1] || diag2[d2] {
                continue
            }
            board[row][col] = 'Q'
            cols[col], diag1[d1], diag2[d2] = true, true, true
            backtrack(row + 1)
            board[row][col] = '.'
            cols[col], diag1[d1], diag2[d2] = false, false, false
        }
    }
    backtrack(0)
    return result
}
```

## Permutations (Backtracking)

```go
func permute(nums []int) [][]int {
    var result [][]int
    used := make([]bool, len(nums))
    var backtrack func(path []int)
    backtrack = func(path []int) {
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
            backtrack(path)
            path = path[:len(path)-1]
            used[i] = false
        }
    }
    backtrack([]int{})
    return result
}
```

## Summary

| Problem | Decision Space | Constraint | Time Complexity |
|---|---|---|---|
| Subsets | Include/exclude | None | O(2ⁿ) |
| Permutations | All orderings | Each used once | O(n·n!) |
| N-Queens | Column per row | No attacks | O(n!) |
