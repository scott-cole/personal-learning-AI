# CHECKPOINT — Search Engine Project

1. What data structure is used to implement the inverted index?
   <details>
   A hash map: `map[string][]Posting` — maps each term to a list of (docID, TF) pairs.
   </details>

2. What does IDF measure?
   <details>
   How rare a term is across all documents. Common words like "the" have low IDF; rare words have high IDF.
   </details>

3. The formula for TF-IDF is:
   <details>
   TF-IDF = TF × IDF. TF = term frequency in document. IDF = log(total docs / docs containing term).
   </details>

4. Why does "the" get a low score even though it appears in many documents?
   <details>
   IDF is low because it appears in almost every document. log(total docs / total docs) ≈ 0.
   </details>

5. How would you support multi-word queries (e.g., "quick brown")?
   <details>
   Search each term separately, intersect the posting lists (AND), sum the TF-IDF scores for each matching document.
   </details>
