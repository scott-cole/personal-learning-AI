# CHECKPOINT — Backtracking

1. What is the core idea of backtracking?
   <details>
   Make a choice, recurse, undo the choice, try the next option. Also called "try and error with undo."
   </details>

2. Why is the N-Queens solution O(n!) and not O(nⁿ)?
   <details>
   The column constraints prune the search space significantly. Each row has at most n choices, but valid choices shrink quickly.
   </details>

3. What data structures are used to track attacked positions in N-Queens?
   <details>
   Three boolean arrays: cols (column), diag1 (row+col), diag2 (row-col+n-1).
   </details>

4. How does the undo step work?
   <details>
   After the recursive call returns, restore the state — remove the queen from the board and mark the columns/diagonals as free.
   </details>

5. How many solutions are there for 8-Queens?
   <details>
   92 distinct solutions.
   </details>
