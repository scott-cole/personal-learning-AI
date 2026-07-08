# DAY 29 — Greedy Algorithms

## Taking the Biggest Slice First

A pizza is cut into slices of different sizes. A greedy person grabs the biggest slice first. Then from what's left, they grab the biggest again. This doesn't always give you the most pizza overall (what if the biggest slice blocks access to two medium ones?), but for certain problems, greedy works perfectly.

A greedy algorithm makes the locally optimal choice at each step, hoping it leads to the globally optimal solution. It works when the problem has the "greedy choice property" — local optima lead to a global optimum.

## Coin Change (Greedy)

With coins [25, 10, 5, 1], greedy works for making change:

```go
func coinChange(coins []int, amount int) map[int]int {
    result := make(map[int]int)
    for _, coin := range coins {
        if amount >= coin {
            count := amount / coin
            result[coin] = count
            amount -= count * coin
        }
    }
    return result
}
```

**Warning**: greedy doesn't work for all coin systems (e.g., coins [1, 3, 4], amount 6 — greedy gives 4+1+1 but optimal is 3+3).

## Interval Scheduling

Given intervals [start, end], select the maximum number of non-overlapping intervals. Strategy: sort by end time, always pick the one that finishes earliest.

```go
type Interval struct {
    Start, End int
}

func intervalScheduling(intervals []Interval) []Interval {
    // Sort by end time
    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i].End < intervals[j].End
    })
    var result []Interval
    lastEnd := 0
    for _, inv := range intervals {
        if inv.Start >= lastEnd {
            result = append(result, inv)
            lastEnd = inv.End
        }
    }
    return result
}
```

## Activity Selection

Similar to interval scheduling: choose the maximum number of activities that don't overlap.

```go
func activitySelection(start, finish []int) []int {
    n := len(start)
    indices := make([]int, n)
    for i := 0; i < n; i++ {
        indices[i] = i
    }
    sort.Slice(indices, func(i, j int) bool {
        return finish[indices[i]] < finish[indices[j]]
    })
    result := []int{indices[0]}
    lastFinish := finish[indices[0]]
    for _, idx := range indices[1:] {
        if start[idx] >= lastFinish {
            result = append(result, idx)
            lastFinish = finish[idx]
        }
    }
    return result
}
```

## When Greedy Works

- **Minimum spanning tree** (Prim's, Kruskal's)
- **Shortest path** (Dijkstra's, with non-negative weights)
- **Huffman coding**
- **Fractional knapsack** (not 0/1 knapsack — that needs DP)

## Summary

| Problem | Greedy Strategy | Works? |
|---|---|---|
| Coin change (canonical) | largest coin first | Yes |
| Interval scheduling | earliest finish first | Yes |
| 0/1 knapsack | highest value/weight | No (needs DP) |
| Minimum spanning tree | smallest edge first | Yes (Kruskal's) |
