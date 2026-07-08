# DAY 21 — Project: Maze Solver

## BFS and DFS on a Grid

This week you learned binary trees, BSTs, heaps, graphs, BFS, and DFS. Now you'll combine BFS and DFS to solve a maze — a grid where 0 is open space and 1 is a wall.

A maze is a graph where each cell is a vertex connected to its open neighbours (up, down, left, right). BFS finds the shortest path. DFS finds *a* path (but not necessarily the shortest).

## The Grid

```
S 0 1 0 0
0 0 1 0 0
1 0 0 0 1
0 0 1 0 E
0 1 0 0 0
```

- S = start
- E = end
- 0 = open path
- 1 = wall

## Approach

- Represent the maze as a `[][]int`.
- BFS: queue of positions, track parent for each cell to reconstruct the path.
- DFS: recursive or stack-based, mark visited.
- Both return the path as coordinates.

## Summary

Building a maze solver uses everything from Week 3:
- Grid as a graph (adjacency implicit via neighbours)
- BFS queue for shortest path
- DFS stack/recursion for exploration
- Visited set to avoid cycles
- Parent tracking for path reconstruction
