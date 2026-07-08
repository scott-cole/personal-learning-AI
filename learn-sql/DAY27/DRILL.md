# DAY27 — Drill: Window Functions

## Setup

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    department VARCHAR(50) NOT NULL,
    salary NUMERIC(10,2) NOT NULL,
    hire_date DATE NOT NULL
);

INSERT INTO employees (name, department, salary, hire_date) VALUES
('Alice', 'Engineering', 120000, '2020-01-15'),
('Bob', 'Engineering', 95000, '2021-03-20'),
('Charlie', 'Engineering', 95000, '2022-07-01'),
('Diana', 'Marketing', 80000, '2021-06-10'),
('Eve', 'Marketing', 75000, '2022-01-15'),
('Frank', 'Marketing', 70000, '2023-02-20'),
('Grace', 'Sales', 90000, '2020-11-01'),
('Heidi', 'Sales', 85000, '2021-08-15'),
('Ivan', 'Sales', 85000, '2022-05-30'),
('Judy', 'Engineering', 110000, '2020-09-01');
```

## Tasks

### 1. Row number by salary (overall)

Assign a unique rank to each employee by salary descending.

### 2. Row number by salary (per department)

Reset the numbering for each department.

### 3. RANK vs DENSE_RANK

Observe the difference when there are ties:

```sql
SELECT name, salary,
    RANK() OVER (ORDER BY salary DESC),
    DENSE_RANK() OVER (ORDER BY salary DESC)
FROM employees;
```

### 4. Top 2 earners per department

Use ROW_NUMBER in a subquery, filter to rn <= 2.

### 5. LAG — salary differences

Show each employee, their salary, and the salary of the previous employee in the same department (order by hire_date).

### 6. Running total of salaries by department

```sql
SELECT name, department, salary,
    SUM(salary) OVER (PARTITION BY department ORDER BY hire_date) AS running_dept_total
FROM employees;
```

### 7. NTILE — quartiles

Divide all employees into 4 salary quartiles.

### 8. Moving average (3-row)

```sql
SELECT name, salary,
    AVG(salary) OVER (ORDER BY salary ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg
FROM employees;
```

## Expected Output for Task 2 (partial)

```
 name   | department  | salary | dept_rank
--------+-------------+--------+----------
 Alice  | Engineering | 120000 |        1
 Judy   | Engineering | 110000 |        2
 Bob    | Engineering |  95000 |        3
 Charlie| Engineering |  95000 |        4
 Diana  | Marketing   |  80000 |        1
 Eve    | Marketing   |  75000 |        2
 Frank  | Marketing   |  70000 |        3
 Grace  | Sales       |  90000 |        1
 Heidi  | Sales       |  85000 |        2
 Ivan   | Sales       |  85000 |        3
```

## Hints

- `PARTITION BY` splits rows into groups; `ORDER BY` within OVER sets the order
- `ROWS BETWEEN ... AND ...` defines the frame
- For task 4, use a subquery: `SELECT * FROM (SELECT ..., ROW_NUMBER() OVER (...) AS rn FROM e) WHERE rn <= 2`
