# CHECKPOINT — Merge Sort and Quicksort

1. What is the time complexity of merge sort in all cases?
   <details>
   O(n log n) — it always splits in half and merges.
   </details>

2. Why is quicksort's worst-case O(n²)?
   <details>
   When the pivot is always the smallest or largest element, partitioning is unbalanced, giving O(n) levels of recursion with O(n) work each.
   </details>

3. What is the space complexity of merge sort?
   <details>
   O(n) — the merge step requires a temporary array of size n.
   </details>

4. Why is quicksort generally preferred over merge sort in practice?
   <details>
   Quicksort is in-place (O(log n) stack space), cache-friendly, and usually faster despite the worst-case risk.
   </details>

5. Is quicksort stable? Why or why not?
   <details>
   No. The partitioning step swaps non-adjacent elements, potentially changing relative order of equal elements.
   </details>
