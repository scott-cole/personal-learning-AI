# CHECKPOINT — Simple Sorts

1. Which of the three simple sorts is stable?
   <details>
   Bubble sort and insertion sort are stable. Selection sort is not.
   </details>

2. What is the best-case time complexity of insertion sort?
   <details>
   O(n) — when the array is already sorted, it only compares each element with its predecessor.
   </details>

3. Why is bubble sort rarely used in practice?
   <details>
   It's O(n²) average and worst case, and has no advantage over insertion sort (which is better for nearly-sorted data).
   </details>

4. What invariant does selection sort maintain?
   <details>
   After i iterations, the first i elements are in their final sorted positions.
   </details>

5. What is the space complexity of all three sorts?
   <details>
   O(1) — they all sort in-place with a constant amount of extra memory.
   </details>
