# CHECKPOINT — Greedy Algorithms

1. What is the greedy choice property?
   <details>
   A globally optimal solution can be arrived at by making a locally optimal (greedy) choice at each step.
   </details>

2. Why doesn't greedy work for 0/1 knapsack?
   <details>
   Taking the highest value/weight ratio item may block combining several lower-ratio items that together give more total value.
   </details>

3. What is the greedy strategy for interval scheduling?
   <details>
   Always pick the interval with the earliest finish time that doesn't overlap with previously selected intervals.
   </details>

4. When does greedy coin change fail?
   <details>
   When the coin denominations don't have the canonical property. E.g., coins [1, 3, 4], amount 6: greedy gives 4+1+1 (3 coins), but optimal is 3+3 (2 coins).
   </details>

5. Give three algorithms that use greedy strategies.
   <details>
   Dijkstra's algorithm (shortest path), Prim's/Kruskal's (minimum spanning tree), Huffman coding.
   </details>
