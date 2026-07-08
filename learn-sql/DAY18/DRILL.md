# DAY18 — Drill: String Manipulation

## Setup

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(200),
    bio TEXT
);

INSERT INTO users (first_name, last_name, email, bio) VALUES
('  Alice ', 'Smith', 'ALICE@EMAIL.COM', 'I love PostgreSQL and data science'),
('Bob', 'Johnson', 'bob.johnson@email.com', NULL),
('CHARLIE', 'Brown', 'charlie@email.com', '  SQL is fun!  '),
('Diana', 'Lee', 'DIANA.LEE@EMAIL.COM', 'Database admin by day, baker by night');
```

## Tasks

### 1. Clean up names

Show first_name with TRIM and INITCAP (title case):

```sql
SELECT INITCAP(TRIM(first_name)) AS clean_name FROM users;
```

### 2. Full name column

Create a full_name column by concatenating first and last names:

```sql
SELECT CONCAT(INITCAP(TRIM(first_name)), ' ', INITCAP(last_name)) AS full_name FROM users;
```

### 3. Lowercase all emails

Show emails in lowercase.

### 4. Extract domain from email

Show the domain part (after @) for each email.

### 5. Replace text in bio

Replace 'PostgreSQL' with 'SQL' in the bio column.

### 6. Email masking

Show first initial + '***@' + domain for privacy:

```sql
SELECT CONCAT(LEFT(LOWER(email), 1), '***@', SUBSTRING(LOWER(email) FROM POSITION('@' IN email) + 1)) AS masked FROM users;
```

### 7. String length

Show the length of each person's bio (using COALESCE for NULLs).

## Expected Output for Task 4

```
     domain
---------------
 email.com
 email.com
 email.com
 email.com
```

## Hints

- Use `SUBSTRING(email FROM POSITION('@' IN email) + 1)` for domain
- `COALESCE(bio, '')` for task 7
- `TRIM` removes leading/trailing spaces
- `INITCAP` capitalizes first letter of each word
