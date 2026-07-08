# DAY11 — Drill: Cross Join and Self-Join

## Setup

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    role VARCHAR(50),
    manager_id INTEGER REFERENCES employees(id)
);

INSERT INTO employees (name, role, manager_id) VALUES
('Alice', 'CEO', NULL),
('Bob', 'VP Engineering', 1),
('Charlie', 'VP Marketing', 1),
('Diana', 'Developer', 2),
('Eve', 'Developer', 2),
('Frank', 'Designer', 3);
```

## Tasks

### 1. Self-join: employee and manager

Write a query showing each employee's name and their manager's name. The CEO should show NULL for manager.

### 2. Find direct reports

Show Alice's direct reports (people who report to employee id 1).

### 3. Cross join practice

```sql
CREATE TABLE toppings (name VARCHAR(20));
CREATE TABLE crusts (name VARCHAR(20));

INSERT INTO toppings VALUES ('Pepperoni'), ('Mushrooms'), ('Olives');
INSERT INTO crusts VALUES ('Thin'), ('Thick'), ('Stuffed');
```

Write a CROSS JOIN to generate all pizza combinations. How many rows?

### 4. Chain self-join: employee → manager → senior manager

Write a query showing: employee name, their manager's name, and their manager's manager name. (Hint: join employees table three times with different aliases.)

### 5. Challenge: Find teammates

Find all pairs of employees who share the same manager. Exclude pairs where the employee is the same person. (Hint: self-join with conditions.)

## Expected Output for Task 5

```
 employee_1 | employee_2 | manager
------------+------------+---------
 Diana      | Eve        | Bob
 Eve        | Diana      | Bob
```

## Hints

- For task 4: join `employees e`, `employees m1`, `employees m2`
- For task 5: use `e1.manager_id = e2.manager_id AND e1.id < e2.id`
- Cross join in task 3 should produce 9 rows
