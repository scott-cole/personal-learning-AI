# DAY 20 — BFS and DFS on Graphs

## A Ripple in a Pond vs Exploring a Cave System

**BFS** is a ripple: you drop a stone in a pond and the ripple expands outward in all directions equally. You see everything at distance 1 before anything at distance 2. This guarantees the shortest path (in unweighted graphs).

**DFS** is exploring a cave system: you follow one passage until it dead-ends, then backtrack and try the next. You go deep before wide. This is useful for topological sorting, cycle detection, and puzzle solving.

## BFS — Breadth-First Search

```go
func BFS(g *Graph, start int) []int {
    visited := make([]bool, g.vertices)
    queue := []int{start}
    visited[start] = true
    var result []int

    for len(queue) > 0 {
        v := queue[0]
        queue = queue[1:]
        result = append(result, v)

        for _, neighbour := range g.edges[v] {
            if !visited[neighbour] {
                visited[neighbour] = true
                queue = append(queue, neighbour)
            }
        }
    }
    return result
}
```

## BFS Shortest Path

```go
func BFSShortest(g *Graph, start, target int) []int {
    visited := make([]bool, g.vertices)
    parent := make([]int, g.vertices)
    for i := range parent {
        parent[i] = -1
    }
    queue := []int{start}
    visited[start] = true

    for len(queue) > 0 {
        v := queue[0]
        queue = queue[1:]
        if v == target {
            return reconstructPath(parent, start, target)
        }
        for _, neighbour := range g.edges[v] {
            if !visited[neighbour] {
                visited[neighbour] = true
                parent[neighbour] = v
                queue = append(queue, neighbour)
            }
        }
    }
    return nil
}

func reconstructPath(parent []int, start, target int) []int {
    path := []int{target}
    for curr := target; curr != start; curr = parent[curr] {
        path = append([]int{parent[curr]}, path...)
    }
    return path
}
```

## DFS — Depth-First Search (Recursive)

```go
func DFS(g *Graph, start int) []int {
    visited := make([]bool, g.vertices)
    var result []int
    dfsHelper(g, start, visited, &result)
    return result
}

func dfsHelper(g *Graph, v int, visited []bool, result *[]int) {
    visited[v] = true
    *result = append(*result, v)
    for _, neighbour := range g.edges[v] {
        if !visited[neighbour] {
            dfsHelper(g, neighbour, visited, result)
        }
    }
}
```

## DFS — Iterative (using a stack)

```go
func DFSIterative(g *Graph, start int) []int {
    visited := make([]bool, g.vertices)
    stack := []int{start}
    var result []int

    for len(stack) > 0 {
        v := stack[len(stack)-1]
        stack = stack[:len(stack)-1]

        if visited[v] {
            continue
        }
        visited[v] = true
        result = append(result, v)

        for _, neighbour := range g.edges[v] {
            if !visited[neighbour] {
                stack = append(stack, neighbour)
            }
        }
    }
    return result
}
```

## BFS vs DFS

| Property | BFS | DFS |
|---|---|---|
| Data structure | Queue | Stack (or recursion) |
| Shortest path | Yes (unweighted) | No |
| Memory | O(width) | O(depth) |
| Use case | Shortest path, level order | Topological sort, cycle detection |
