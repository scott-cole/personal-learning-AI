# DAY15: Build a REST API Client in Go

---

## The anecdote: "This is your first professional tool"

Everything so far has been practice. Today you build something real: a Go program that talks to a real REST API, handles errors, paginates results, and produces useful output.

This is the difference between knowing HTTP and **using** HTTP.

---

## The project: GitHub API client

Build a command-line tool called `gh-client` that:

1. Fetches public user profiles
2. Lists repos for a user
3. Searches for repositories
4. Displays results in a formatted way

---

## Step 1: A reusable API client

Create `client.go`:

```go
package main

import (
    "encoding/json"
    "fmt"
    "io"
    "net/http"
    "time"
)

type Client struct {
    baseURL    string
    httpClient *http.Client
    token      string
}

func NewClient(token string) *Client {
    return &Client{
        baseURL: "https://api.github.com",
        httpClient: &http.Client{Timeout: 10 * time.Second},
        token: token,
    }
}

func (c *Client) doRequest(method, path string, body io.Reader) (*http.Response, error) {
    req, err := http.NewRequest(method, c.baseURL+path, body)
    if err != nil {
        return nil, fmt.Errorf("creating request: %w", err)
    }
    req.Header.Set("Accept", "application/vnd.github.v3+json")
    req.Header.Set("User-Agent", "gh-client/1.0")
    if c.token != "" {
        req.Header.Set("Authorization", "Bearer "+c.token)
    }
    resp, err := c.httpClient.Do(req)
    if err != nil {
        return nil, fmt.Errorf("sending request: %w", err)
    }
    if resp.StatusCode < 200 || resp.StatusCode >= 300 {
        body, _ := io.ReadAll(resp.Body)
        resp.Body.Close()
        return nil, fmt.Errorf("HTTP %d: %s", resp.StatusCode, body)
    }
    return resp, nil
}
```

---

## Step 2: Type definitions

```go
type User struct {
    Login     string `json:"login"`
    Name      string `json:"name"`
    PublicRepos int   `json:"public_repos"`
    Followers   int   `json:"followers"`
    Following   int   `json:"following"`
    CreatedAt string `json:"created_at"`
    HTMLURL   string `json:"html_url"`
}

type Repo struct {
    Name        string `json:"name"`
    Description string `json:"description"`
    Stars       int    `json:"stargazers_count"`
    Forks       int    `json:"forks_count"`
    Language    string `json:"language"`
    HTMLURL     string `json:"html_url"`
}

type SearchResult struct {
    Items      []Repo `json:"items"`
    TotalCount int    `json:"total_count"`
}
```

---

## Step 3: Methods

```go
func (c *Client) GetUser(username string) (*User, error) {
    resp, err := c.doRequest("GET", "/users/"+username, nil)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    var user User
    if err := json.NewDecoder(resp.Body).Decode(&user); err != nil {
        return nil, fmt.Errorf("decoding response: %w", err)
    }
    return &user, nil
}

func (c *Client) ListRepos(username string) ([]Repo, error) {
    resp, err := c.doRequest("GET", "/users/"+username+"/repos?per_page=10", nil)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    var repos []Repo
    if err := json.NewDecoder(resp.Body).Decode(&repos); err != nil {
        return nil, fmt.Errorf("decoding response: %w", err)
    }
    return repos, nil
}

func (c *Client) SearchRepos(query string) (*SearchResult, error) {
    resp, err := c.doRequest("GET", "/search/repositories?q="+query, nil)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    var result SearchResult
    if err := json.NewDecoder(resp.Body).Decode(&result); err != nil {
        return nil, fmt.Errorf("decoding response: %w", err)
    }
    return &result, nil
}
```

---

## Step 4: Main program

Create `main.go`:

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

func main() {
    token := os.Getenv("GITHUB_TOKEN")
    client := NewClient(token)

    switch flag.Arg(0) {
    case "user":
        if flag.NArg() < 2 {
            fmt.Println("Usage: gh-client user <username>")
            os.Exit(1)
        }
        user, err := client.GetUser(flag.Arg(1))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(1)
        }
        fmt.Printf("User:     %s (%s)\n", user.Name, user.Login)
        fmt.Printf("Repos:    %d\n", user.PublicRepos)
        fmt.Printf("Followers: %d\n", user.Followers)
        fmt.Printf("Profile:  %s\n", user.HTMLURL)

    case "repos":
        if flag.NArg() < 2 {
            fmt.Println("Usage: gh-client repos <username>")
            os.Exit(1)
        }
        repos, err := client.ListRepos(flag.Arg(1))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(1)
        }
        for _, r := range repos {
            fmt.Printf("* %s (%d ★) - %s\n", r.Name, r.Stars, r.Language)
        }

    case "search":
        if flag.NArg() < 2 {
            fmt.Println("Usage: gh-client search <query>")
            os.Exit(1)
        }
        result, err := client.SearchRepos(flag.Arg(1))
        if err != nil {
            fmt.Println("Error:", err)
            os.Exit(1)
        }
        fmt.Printf("Found %d results:\n", result.TotalCount)
        for _, r := range result.Items {
            fmt.Printf("* %s (%d ★)\n", r.Name, r.Stars)
        }

    default:
        fmt.Println("Usage: gh-client <user|repos|search> [args...]")
        os.Exit(1)
    }
}
```

---

## Senior mindset: "Your API client is documentation"

A well-structured API client is the best documentation for your API. Types, methods, and error handling tell other developers exactly how to use it.

Every production system should have a **dedicated client package** — don't make HTTP calls all over your codebase. Centralise them.

---

## Summary

| File | Purpose |
|------|---------|
| `client.go` | Reusable HTTP client with auth, error handling |
| `types.go` | Data structures matching the API responses |
| `methods.go` | Business logic (GetUser, ListRepos, SearchRepos) |
| `main.go` | CLI entry point with argument parsing |
