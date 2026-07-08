# CHECKPOINT — BFS and DFS

1. What data structure does BFS use?
   <details>
   A queue — FIFO ensures nodes are visited in order of distance from the start.
   </details>

2. Why does BFS find the shortest path in an unweighted graph?
   <details>
   It visits nodes in increasing order of distance. The first time it reaches the target is the shortest path.
   </details>

3. What data structure does DFS use (iterative)?
   <details>
   A stack — LIFO ensures deep exploration of one branch before others.
   </details>

4. What is the visited set for?
   <details>
   To prevent visiting the same node twice, which would cause infinite loops (especially in cyclic graphs).
   </details>

5. Which traversal uses more memory for a wide graph vs a deep graph?
   <details>
   BFS uses more memory for wide graphs (queue holds all nodes at the current level). DFS uses more for deep graphs (stack holds the path from root to current node).
   </details>
