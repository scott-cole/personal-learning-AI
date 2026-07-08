# CHECKPOINT — Recursion

1. What are the two essential parts of a recursive function?
   <details>
   A base case (stops recursion) and a recursive case (calls itself with smaller input).
   </details>

2. What is the time complexity of naive recursive Fibonacci?
   <details>
   O(2ⁿ) — each call branches into two, creating an exponential tree.
   </details>

3. How many subsets does a set of n elements have?
   <details>
   2ⁿ — each element is either included or excluded.
   </details>

4. How many permutations does an n-element set have?
   <details>
   n! — n choices for the first position, n-1 for the second, etc.
   </details>

5. What is the recursion limit in Go?
   <details>
   There's no fixed limit — it depends on stack size. Typically ~1 billion calls on a large stack, but recursion depth beyond ~10,000 may overflow on smaller stacks. Iteration is safer for deep recursion.
   </details>
