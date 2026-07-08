# DRILL — In-Memory Search Engine

## Task

Build an in-memory search engine with inverted index and TF-IDF ranking.

## Starter Code

Create `main.go`:

```go
package main

import (
    "fmt"
    "math"
    "sort"
    "strings"
)

type Posting struct {
    DocID int
    TF    float64
}

type Index struct {
    docs []string        // document contents
    idx  map[string][]Posting  // term → postings
}

// TODO: Tokenize text into lowercase words.
func Tokenize(text string) []string {
    return nil
}

// TODO: Compute TF for each term in a document.
func ComputeTF(terms []string) map[string]float64 {
    return nil
}

// TODO: Add a document to the index.
func (idx *Index) AddDocument(id int, text string) {
}

// TODO: Build the inverted index from all documents.
func (idx *Index) Build(docs []string) {
    idx.docs = docs
    idx.idx = make(map[string][]Posting)
    for i, doc := range docs {
        idx.AddDocument(i, doc)
    }
}

// TODO: Compute IDF for a term.
func (idx *Index) IDF(term string) float64 {
    return 0
}

// TODO: Search for a term, return top K doc IDs sorted by TF-IDF score.
func (idx *Index) Search(term string, topK int) []struct {
    DocID int
    Score float64
} {
    return nil
}

func main() {
    docs := []string{
        "the quick brown fox jumps over the lazy dog",
        "the quick brown cat jumps over the lazy rat",
        "the slow brown bear sits under a tree",
        "a fast white fox chases a quick brown rabbit",
    }

    index := &Index{}
    index.Build(docs)

    for _, term := range []string{"fox", "brown", "quick", "lazy", "tree"} {
        fmt.Printf("\nSearch '%s':\n", term)
        results := index.Search(term, 3)
        if len(results) == 0 {
            fmt.Println("  No results")
            continue
        }
        for _, r := range results {
            fmt.Printf("  doc %d: score=%.4f  (%s)\n", r.DocID, r.Score, docs[r.DocID])
        }
    }
}
```

## Expected Output

```
Search 'fox':
  doc 0: score=...  (the quick brown fox jumps over the lazy dog)
  doc 3: score=...  (a fast white fox chases a quick brown rabbit)

Search 'brown':
  doc 0: score=...  ...
  doc 1: score=...  ...
  doc 2: score=...  ...
  doc 3: score=...  ...

Search 'tree':
  doc 2: score=...  (the slow brown bear sits under a tree)
```

## Hints

- TF = count of term in doc / total terms in doc.
- IDF = log(total docs / number of docs containing term). Add 1 to denominator to avoid division by zero.
- Sort results by descending score, return top K.
- Use `strings.Fields` and `strings.ToLower` for tokenization.

## Run

```bash
go run main.go
```
