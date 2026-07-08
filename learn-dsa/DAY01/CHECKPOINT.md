# CHECKPOINT — Big O Notation

1. What is the time complexity of accessing `arr[5]` in a slice?
   <details>
   O(1) — constant time. Array access by index is a direct memory address calculation.
   </details>

2. What is the time complexity of a binary search on a sorted array of size n?
   <details>
   O(log n). Each step halves the search space.
   </details>

3. If an algorithm takes 3n² + 2n + 1 operations, what is its Big O?
   <details>
   O(n²). Drop constants and lower-order terms.
   </details>

4. What is the space complexity of creating a copy of an n-element slice?
   <details>
   O(n) — you allocate n new elements.
   </details>

5. Which grows faster: O(n log n) or O(n²)?
   <details>
   O(n²) grows faster. For large n, quadratic dominates linearithmic.
   </details>
