# CHECKPOINT — In-Place Operations

1. What is the space complexity of an in-place algorithm?
   <details>
   O(1) — only a constant amount of extra space.
   </details>

2. How does the triple-reverse rotate work?
   <details>
   Reverse the whole array, then reverse first k elements, then reverse the rest. End result is a right rotation.
   </details>

3. In the Dutch National Flag algorithm, why doesn't `mid` increment on case 2?
   <details>
   Because we swapped an unknown value from `high` into `mid`. We need to re-examine it.
   </details>

4. What does `removeElement` return?
   <details>
   The length of the modified array (elements not equal to val). The first k elements contain the result.
   </details>

5. Why is in-place reversal more memory efficient than creating a new array?
   <details>
   It uses a single temporary variable for swaps instead of O(n) space for a copy.
   </details>
