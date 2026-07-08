# DAY15: Drill — "Build and run gh-client"

Today you implement and test the GitHub API client from the lesson.

## Step 1 — Create the files

Create three files in your DAY15 folder:

1. `client.go` — the HTTP client (from the lesson)
2. `types.go` — User, Repo, SearchResult structs
3. `main.go` — CLI entry point

## Step 2 — Test without authentication (rate limits apply)

```bash
go run main.go user google
# Expected: User info for Google

go run main.go repos google
# Expected: List of repos (rate limited to 60/hour)

go run main.go search "web framework language:go"
# Expected: Search results
```

## Step 3 — Add authentication for higher rate limits

```bash
# Set your token (get one from github.com/settings/tokens)
export GITHUB_TOKEN=ghp_xxxxxxxxxxxx

go run main.go user google
# Now you get 5000 requests/hour instead of 60
```

## Step 4 — Add a useful feature

Choose one feature to add:

1. **Sort repos by stars** — add `?sort=stars` to the repos endpoint
2. **Pagination flag** — add `--page` and `--per-page` flags
3. **Output formatting** — add `--json` flag to output raw JSON
4. **Error messages** — improve error messages with status code and body

## Step 5 — Debug a failing request

```bash
# What happens if you search for an empty query?
go run main.go search ""

# What about a user that doesn't exist?
go run main.go user thisuserdoesnotexist12345

# What about network failure?
# Disconnect from WiFi and try:
go run main.go user google
```

## Deliverables

1. Your three source files (client.go, types.go, main.go)
2. Output from `go run main.go user google`
3. Which feature did you add in Step 4?
4. What error did you get for the non-existent user?

## Hints

<details>
<summary>Click for hints</summary>

- GitHub API returns `404 Not Found` for non-existent users
- Empty queries return `422 Unprocessable Entity`
- Network failures return a Go error (not an HTTP error)
- `go build` creates a binary you can run from anywhere
- `GITHUB_TOKEN` env var is read in `main()`
</details>
