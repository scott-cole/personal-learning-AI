# DAY24 — Drill: Apply Constraints

## Setup

```sql
CREATE DATABASE constraints_lab;
\c constraints_lab
```

## Tasks

### 1. Create a `members` table with constraints

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| email | VARCHAR(200) | NOT NULL, UNIQUE |
| age | INTEGER | CHECK age >= 18 |
| status | VARCHAR(20) | DEFAULT 'active' |
| joined_at | DATE | DEFAULT CURRENT_DATE |

### 2. Test NOT NULL

Try inserting a row with NULL email. What happens?

### 3. Test UNIQUE

Insert a row, then try inserting another with the same email.

### 4. Test CHECK

Try inserting a member with age = 15. What happens?

### 5. Test DEFAULT

Insert a row with only email and age. Check that status defaults to 'active'.

### 6. Add a constraint to existing table

```sql
ALTER TABLE members ADD CONSTRAINT members_email_format CHECK (email LIKE '%@%');
```

Test: try inserting an email without @.

### 7. Multi-column UNIQUE

```sql
CREATE TABLE subscriptions (
    id SERIAL PRIMARY KEY,
    member_id INTEGER REFERENCES members(id),
    service VARCHAR(100) NOT NULL,
    tier VARCHAR(20) NOT NULL,
    UNIQUE (member_id, service)  -- One subscription per service per member
);
```

Test inserting a duplicate (member_id, service) pair.

## Hints

- Error messages tell you exactly which constraint was violated
- `DROP TABLE` and recreate if you mess up the schema
- `ALTER TABLE ... ADD CONSTRAINT` is useful for modifying existing tables
- The `LIKE '%@%'` check is crude but works for demonstration
