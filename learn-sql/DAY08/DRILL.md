# DAY08 — Drill: Build a PK/FK Schema

## Setup

```sql
CREATE DATABASE hospital;
\c hospital
```

## Tasks

### 1. Create a `departments` table

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| name | VARCHAR(100) | NOT NULL, UNIQUE |
| building | VARCHAR(50) | NOT NULL |

### 2. Create a `doctors` table

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| name | VARCHAR(100) | NOT NULL |
| department_id | INTEGER | FK → departments(id), ON DELETE SET NULL |

### 3. Create a `patients` table

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| name | VARCHAR(100) | NOT NULL |
| primary_doctor_id | INTEGER | FK → doctors(id), ON DELETE SET NULL |

### 4. Create an `appointments` table

| Column | Type | Constraints |
|--------|------|-------------|
| id | SERIAL | PK |
| patient_id | INTEGER | FK → patients(id), ON DELETE CASCADE |
| doctor_id | INTEGER | FK → doctors(id), ON DELETE CASCADE |
| appointment_date | TIMESTAMP | NOT NULL |
| reason | TEXT | |

### 5. Insert data

Insert 3 departments, 4 doctors, 5 patients, and 8 appointments.

### 6. Test referential integrity

Try to:

```sql
-- What happens?
DELETE FROM doctors WHERE id = 1;

-- What happens?
DELETE FROM patients WHERE id = 1;

-- What happens?
INSERT INTO appointments (patient_id, doctor_id) VALUES (999, 1);
```

## Hints

- Insert in dependency order: departments → doctors → patients → appointments
- Test `ON DELETE CASCADE` vs `ON DELETE SET NULL` by deleting a parent row
- The last insert should fail with a foreign key violation
