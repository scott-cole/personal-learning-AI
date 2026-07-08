# DAY 30 — Final Project: In-Memory Search Engine

## Inverted Index

This is the capstone project. You'll build an in-memory search engine with:
- **Tokenization**: split documents into terms
- **Inverted index**: map term → list of (docID, frequency)
- **TF-IDF ranking**: rank results by term frequency × inverse document frequency
- **Query**: search by single term and return top K results

This project ties together hash maps (inverted index), slices (document storage), sorting (rank results), and the algorithmic thinking from all 30 days.

## How TF-IDF Works

**TF (Term Frequency)**: how often a term appears in a document.
```
TF = count_of_term_in_doc / total_terms_in_doc
```

**IDF (Inverse Document Frequency)**: how rare a term is across all documents.
```
IDF = log(total_docs / docs_containing_term)
```

**TF-IDF score** = TF × IDF. A term is important to a document if it appears often in that document (high TF) but rarely elsewhere (high IDF).

## Architecture

```
Documents:
  doc0: "the quick brown fox"
  doc1: "the lazy brown dog"
  doc2: "the quick brown cat"

Inverted Index:
  "the"   → [{0,1}, {1,1}, {2,1}]
  "quick" → [{0,1}, {2,1}]
  "brown" → [{0,1}, {1,1}, {2,1}]
  "fox"   → [{0,1}]
  "lazy"  → [{1,1}]
  "dog"   → [{1,1}]
  "cat"   → [{2,1}]

Search "quick" → docs 0 and 2, ranked by TF-IDF.
```

## Summary

This project uses:
- Hash maps (`map[string][]Posting`)
- Slices and iteration
- Sorting (`sort.Slice`)
- Math (`math.Log`)
- String processing (`strings.Fields`, `strings.ToLower`)
