# DAY26 — Checkpoint

## Questions

**1. What problem does normalization solve?**

<details>
<summary>Answer</summary>
It reduces data redundancy, prevents update anomalies, and improves data integrity by organising data into related tables.
</details>

**2. What is 1NF and what violates it?**

<details>
<summary>Answer</summary>
1NF requires atomic columns (no lists or arrays), consistent column types, and unique rows. A column storing comma-separated values violates 1NF.
</details>

**3. What is a transitive dependency in 3NF?**

<details>
<summary>Answer</summary>
A non-key column depends on another non-key column rather than directly on the primary key. E.g., `customer_phone` depends on `customer_id`, not on `order_id`.
</details>

**4. When would you intentionally denormalize?**

<details>
<summary>Answer</summary>
For performance reasons: to avoid expensive JOINs on frequently accessed data, for reporting/dashboard tables, or for read-heavy workloads where write anomalies are acceptable.
</details>

**5. Given a table `enrollments(student_id, student_name, course_id, course_name)`, which normal form violation exists?**

<details>
<summary>Answer</summary>
This violates 2NF: `student_name` depends only on `student_id` (part of the composite key if PK is `(student_id, course_id)`), and `course_name` depends only on `course_id`.
</details>
