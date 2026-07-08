# DAY21: Week 3 Review — "Write 5 Queries From Memory"

Week 3 covered: SQL, database/sql, migrations, SQL repository, transactions, error wrapping.

Today is a **review checkpoint**. No new concepts.

## Your task

1. Take the closed-book quiz in CHECKPOINT.md
2. Build a small end-to-end program that connects everything from Week 3

## Mini-project: URL Shortener Stats CLI

Build a CLI tool that connects to the URL shortener database and prints stats:

```
$ go run main.go
All URLs:
abc123 → https://example.com (5 visits)
def456 → https://google.com (3 visits)

Total URLs: 2
Total visits: 8
```

## Structure

```
DAY21/
├── go.mod
├── main.go              → CLI tool that reads from SQLite
└── stats.db             → created by sqlite3 (copy from DAY16 if you have it)
```

## Requirements

1. Open a SQLite database file (passed as arg or hardcoded as `./url_shortener.db`)
2. Query all URLs with their access counts
3. Print a formatted summary
4. Handle the case where the database doesn't exist

## Stretch goal

Add a `-insert` flag that lets you add a URL from the command line:

```bash
go run main.go -insert "https://example.com"
```

## Run it

```bash
cd ~/Dev/learn-go/DAY21
go mod init learn-go/DAY21
go get github.com/mattn/go-sqlite3
go run main.go
```
