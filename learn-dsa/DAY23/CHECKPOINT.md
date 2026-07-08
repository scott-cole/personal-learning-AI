# CHECKPOINT — Binary Search

1. What condition must the input satisfy for binary search to work?
   <details>
   The input must be sorted (ascending or descending).
   </details>

2. What is the time complexity of binary search?
   <details>
   O(log n) — each step halves the search space.
   </details>

3. Why use `mid = lo + (hi-lo)/2` instead of `mid = (lo+hi)/2`?
   <details>
   To avoid integer overflow when lo+hi exceeds the maximum int value.
   </details>

4. In the rotated array search, how do you know which half is sorted?
   <details>
   If `nums[lo] <= nums[mid]`, the left half is sorted. Otherwise, the right half is sorted.
   </details>

5. How do you find the first occurrence of a target with binary search?
   <details>
   When you find the target, don't return immediately — set `hi = mid - 1` and continue searching left.
   </details>
