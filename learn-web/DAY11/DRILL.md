# DAY11: Drill — "Paginate the GitHub API"

Learn to navigate large datasets using the GitHub API's pagination.

## Part 1 — Explore pagination headers

```bash
# Fetch the first page of golang/go issues
curl -I "https://api.github.com/repos/golang/go/issues?per_page=5"
```

Look for the `Link` header in the response. It contains links to `next`, `last`, `first`, and `prev` pages.

```text
Link: <https://api.github.com/repos/golang/go/issues?per_page=5&page=2>; rel="next",
      <https://api.github.com/repos/golang/go/issues?per_page=5&page=119>; rel="last"
```

## Part 2 — Follow the pages

```bash
# Page 1
curl -s "https://api.github.com/repos/golang/go/issues?per_page=3" | python3 -m json.tool | head -20

# Page 2
curl -s "https://api.github.com/repos/golang/go/issues?per_page=3&page=2" | python3 -m json.tool | head -20

# Page 3
curl -s "https://api.github.com/repos/golang/go/issues?per_page=3&page=3" | python3 -m json.tool | head -20
```

## Part 3 — Filter + paginate

```bash
# Find all open bugs, page by page
curl -s "https://api.github.com/repos/golang/go/issues?state=open&labels=bug&per_page=5&page=1"
```

## Part 4 — Write a pagination loop in Go

Create `main.go`:

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "time"
)

type Issue struct {
    Number    int    `json:"number"`
    Title     string `json:"title"`
    State     string `json:"state"`
}

func fetchIssues(url string) ([]Issue, string, error) {
    client := &http.Client{Timeout: 10 * time.Second}
    resp, err := client.Get(url)
    if err != nil {
        return nil, "", err
    }
    defer resp.Body.Close()

    var issues []Issue
    json.NewDecoder(resp.Body).Decode(&issues)

    // Extract next page URL from Link header
    link := resp.Header.Get("Link")
    return issues, link, nil
}

func main() {
    url := "https://api.github.com/repos/golang/go/issues?per_page=5&page=1"
    for url != "" {
        issues, link, _ := fetchIssues(url)
        for _, issue := range issues {
            fmt.Printf("#%d [%s] %s\n", issue.Number, issue.State, issue.Title)
        }
        fmt.Printf("--- %d issues on this page ---\n\n", len(issues))
        // Parse next URL from link header (simplified)
        url = "" // break to avoid infinite loop
    }
}
```

## Part 5 — Count total issues

Use the `Link` header to find the last page and calculate the total number of pages.

## Deliverables

1. What does the `Link` header look like?
2. How many pages of open issues does `golang/go` have?
3. Your Go program output (at least 10 issues across pages)

## Hints

<details>
<summary>Click for hints</summary>

- The `Link` header format: `<url>; rel="next"` — you need to parse it
- GitHub's API docs: developer.github.com
- Without authentication, GitHub rate limits to 60 requests/hour
- Add `-H "Accept: application/vnd.github.v3+json"` for the v3 API
</details>
