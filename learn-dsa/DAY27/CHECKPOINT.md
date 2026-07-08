# CHECKPOINT — Dynamic Programming

1. What is the key difference between memoization and tabulation?
   <details>
   Memoization is top-down (recursive + cache). Tabulation is bottom-up (iterative table).
   </details>

2. What is the time complexity of the DP knapsack solution?
   <details>
   O(n·C) where n is the number of items and C is the capacity.
   </details>

3. What property must a problem have for DP to work?
   <details>
   Optimal substructure (optimal solution contains optimal solutions to subproblems) and overlapping subproblems.
   </details>

4. Why is naive recursive Fibonacci O(2ⁿ) but DP Fibonacci O(n)?
   <details>
   Naive recomputes the same subproblems exponentially. DP solves each once.
   </details>

5. In 0/1 knapsack, what does dp[i][w] represent?
   <details>
   The maximum value achievable using the first i items with capacity w.
   </details>
