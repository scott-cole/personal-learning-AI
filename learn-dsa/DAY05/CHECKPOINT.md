# CHECKPOINT — Prefix Sums

1. What does `prefix[i]` represent in a standard 1D prefix sum of length n+1?
   <details>
   The sum of elements `nums[0..i-1]`. prefix[0] = 0, prefix[n] = total sum.
   </details>

2. How do you compute `sum(l, r)` using prefix sums?
   <details>
   `prefix[r+1] - prefix[l]`. Both bounds inclusive.
   </details>

3. What is the time to build a prefix sum array?
   <details>
   O(n) — a single pass.
   </details>

4. In the 2D prefix sum formula, why do you add back `pref[r1][c1]`?
   <details>
   Because it was subtracted twice (once in each subtraction). The inclusion-exclusion principle corrects the double-count.
   </details>

5. How does the hash map trick count subarrays summing to k?
   <details>
   It stores frequencies of prefix sums. For each current sum, `sum - k` tells you how many earlier subarrays end at the current position.
   </details>
