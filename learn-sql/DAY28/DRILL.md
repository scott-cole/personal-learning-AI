# DAY28 — Drill: CTE Practice

## Setup

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50),
    salary NUMERIC(10,2),
    manager_id INTEGER REFERENCES employees(id)
);

INSERT INTO employees (id, name, department, salary, manager_id) VALUES
(1, 'Alice', 'Executive', 200000, NULL),
(2, 'Bob', 'Engineering', 120000, 1),
(3, 'Charlie', 'Engineering', 95000, 2),
(4, 'Diana', 'Marketing', 110000, 1),
(5, 'Eve', 'Marketing', 80000, 4),
(6, 'Frank', 'Engineering', 90000, 2),
(7, 'Grace', 'Marketing', 75000, 4);
```

## Tasks

### 1. Simple CTE — department totals

Write a CTE that calculates total salary per department. Then SELECT from it to find departments with total > 150000.

### 2. Multiple CTEs — top earners plus dept averages

Use one CTE for department averages, another for top earners (salary > dept average), then SELECT the combination.

### 3. Recursive CTE — org chart

Generate the full org hierarchy with levels, starting from Alice (id=1). Show name, level, and a path column.

### 4. Recursive CTE — date series

Generate all dates in January 2024.

### 5. Modifying CTE — archive old records

```sql
CREATE TABLE logs (
    id SERIAL PRIMARY KEY,
    message TEXT,
    created_at DATE DEFAULT CURRENT_DATE
);

INSERT INTO logs (message, created_at) VALUES
('Error 1', '2023-01-01'),
('Error 2', '2023-06-15'),
('Info 1', '2024-01-01');

CREATE TABLE logs_archive (LIKE logs);
```

Use a CTE to move logs older than 2024 into the archive and delete from logs.

## Expected Output for Task 3

```
 id |  name   | level
----+---------+-------
  1 | Alice   |     1
  2 | Bob     |     2
  4 | Diana   |     2
  3 | Charlie |     3
  6 | Frank   |     3
  5 | Eve     |     3
  7 | Grace   |     3
```

## Hints

- `WITH RECURSIVE` requires `UNION ALL`
- Base case (non-recursive) first, then recursive part
- Recursive part joins the CTE name to the table
- Always include a termination condition (WHERE clause) in recursive CTEs
