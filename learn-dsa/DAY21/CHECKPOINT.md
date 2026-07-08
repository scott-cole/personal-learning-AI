# CHECKPOINT — Maze Solver Project

1. Why does BFS guarantee the shortest path in the maze?
   <details>
   BFS visits cells in order of distance from the start. The first time it reaches the end is the shortest path.
   </details>

2. How do you avoid revisiting cells during the search?
   <details>
   Use a visited set (e.g., `map[Point]bool` or a 2D bool array). Mark cells when they're first discovered.
   </details>

3. What is the time complexity of BFS on an R×C grid?
   <details>
   O(R·C) — each cell is visited at most once.
   </details>

4. How do you reconstruct the path after BFS finds the target?
   <details>
   Use a parent map. When you discover a cell, record which cell you came from. After reaching the target, follow parents back to the start.
   </details>

5. Why might DFS find a longer path than BFS?
   <details>
   DFS goes deep first — it may take a suboptimal route. It explores one branch fully before backtracking, so it finds *a* path, not necessarily the shortest.
   </details>
