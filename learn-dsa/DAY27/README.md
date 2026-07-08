# DAY 27 — Dynamic Programming

## Remembering Where You Buried Treasure

Imagine burying gold in a field with a map. You don't dig the same spot twice — you mark it on your map. Next time you need that spot, you just look at the map instead of digging again.

Dynamic Programming (DP) is the same: solve a problem by breaking it into overlapping subproblems, solving each once, and storing the result. There are two approaches:

- **Memoization** (top-down): recursive with a cache
- **Tabulation** (bottom-up): iterative with a table

## Fibonacci with Memoization

```go
func fibMemo(n int) int {
    memo := make([]int, n+1)
    var f func(int) int
    f = func(k int) int {
        if k <= 1 {
            return k
        }
        if memo[k] != 0 {
            return memo[k]
        }
        memo[k] = f(k-1) + f(k-2)
        return memo[k]
    }
    return f(n)
}
```

## Fibonacci with Tabulation

```go
func fibTab(n int) int {
    if n <= 1 {
        return n
    }
    dp := make([]int, n+1)
    dp[0], dp[1] = 0, 1
    for i := 2; i <= n; i++ {
        dp[i] = dp[i-1] + dp[i-2]
    }
    return dp[n]
}
```

## 0/1 Knapsack

Given items with weights and values, a knapsack capacity, maximise the value you can carry (each item taken at most once).

```go
func knapsack(weights []int, values []int, capacity int) int {
    n := len(weights)
    dp := make([][]int, n+1)
    for i := range dp {
        dp[i] = make([]int, capacity+1)
    }

    for i := 1; i <= n; i++ {
        for w := 0; w <= capacity; w++ {
            if weights[i-1] <= w {
                // max(skip, take)
                take := values[i-1] + dp[i-1][w-weights[i-1]]
                if take > dp[i-1][w] {
                    dp[i][w] = take
                } else {
                    dp[i][w] = dp[i-1][w]
                }
            } else {
                dp[i][w] = dp[i-1][w]
            }
        }
    }
    return dp[n][capacity]
}
```

## Memoization vs Tabulation

| Property | Memoization | Tabulation |
|---|---|---|
| Approach | Top-down (recursive) | Bottom-up (iterative) |
| Implementation | Easier (natural recursion) | More explicit |
| Space | Can be O(n) stack + cache | Just the table |
| Speed | Slight overhead (function calls) | Faster (no recursion) |
| Skip unnecessary states | Yes | No (fills all) |
