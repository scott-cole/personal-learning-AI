# DAY15: Drill — "Write SQL Queries"

No Go today. Just SQL. You'll create a table, insert data, and query it.

## Setup

```bash
# Install sqlite if you don't have it
brew install sqlite

# Create a database
cd ~/Dev/learn-go/DAY15
sqlite3 url_shortener.db
```

Now you're in the SQLite prompt (`sqlite>`). Type `.exit` to quit.

## Step 1: Create the table

```sql
CREATE TABLE urls (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    code TEXT NOT NULL UNIQUE,
    original_url TEXT NOT NULL,
    access_count INTEGER NOT NULL DEFAULT 0,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);
```

## Step 2: Insert data

```sql
INSERT INTO urls (code, original_url) VALUES ('abc123', 'https://example.com');
INSERT INTO urls (code, original_url) VALUES ('def456', 'https://google.com');
INSERT INTO urls (code, original_url) VALUES ('ghi789', 'https://github.com');
```

## Step 3: Query

Write SQL for each of these:

1. Get all URLs
2. Get the original URL for code `abc123`
3. Count how many URLs are stored
4. Get all URLs where the original URL contains `google`
5. Get the URL with the most access_count (once you've added some)

## Step 4: Update and delete

1. Increment access_count for `abc123` by 1
2. Delete the URL with code `ghi789`

## Expected results

```sql
-- Run these to verify
SELECT * FROM urls;
-- Should show 3 rows

SELECT original_url FROM urls WHERE code = 'abc123';
-- https://example.com

SELECT COUNT(*) FROM urls;
-- 3
```

## Your task

Copy these queries into CHECKPOINT.md answers so you have them for reference. Tomorrow you'll run them from Go.

## Run it

```bash
cd ~/Dev/learn-go/DAY15
sqlite3 url_shortener.db
```
