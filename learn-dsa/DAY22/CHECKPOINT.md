# CHECKPOINT — Hash Maps

1. What problem does separate chaining solve?
   <details>
   Hash collisions — two different keys that hash to the same bucket index.
   </details>

2. What is the load factor?
   <details>
   The ratio of stored entries to bucket capacity (size / capacity). Used to decide when to resize.
   </details>

3. What is the time complexity of Get in a well-distributed hash map?
   <details>
   O(1) average case. Each bucket has O(1) entries on average.
   </details>

4. What makes a good hash function?
   <details>
   Deterministic, fast, and produces a uniform distribution of bucket indices.
   </details>

5. What happens when the hash map grows beyond its load factor?
   <details>
   It resizes — creates a new, larger bucket array and rehashes all entries. This is O(n) but infrequent.
   </details>
