# DAY19 — Drill: Date/Time Analytics

## Setup

```sql
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    event_name VARCHAR(200) NOT NULL,
    event_date DATE NOT NULL,
    event_time TIMESTAMP NOT NULL DEFAULT NOW(),
    duration_minutes INTEGER
);

INSERT INTO events (event_name, event_date, event_time, duration_minutes) VALUES
('Product Launch', '2024-01-15', '2024-01-15 09:00:00', 120),
('Team Meeting', '2024-01-15', '2024-01-15 14:00:00', 60),
('Webinar', '2024-01-20', '2024-01-20 11:00:00', 90),
('Conference', '2024-02-05', '2024-02-05 08:00:00', 480),
('Workshop', '2024-02-10', '2024-02-10 10:00:00', 180),
('Networking', '2024-03-01', '2024-03-01 17:00:00', 120),
('Training', '2024-03-15', '2024-03-15 09:30:00', 240);
```

## Tasks

### 1. Current date/time queries

Run each of these:

```sql
SELECT NOW(), CURRENT_DATE, CURRENT_TIME;
```

### 2. Extract day of week

Find all events that happen on a Friday.

### 3. Events in the last 30 days (from '2024-03-15')

```sql
SELECT * FROM events WHERE event_date > CURRENT_DATE - INTERVAL '30 days';
```

But use the literal date `'2024-03-15'` as "today."

### 4. Monthly event count

Use DATE_TRUNC to group events by month and count them.

### 5. Event duration in hours

Show event_name and duration in hours (duration_minutes / 60.0).

### 6. End time calculation

Show event_name, start time, and end time (event_time + duration_minutes).

### 7. Age calculation

How many days ago was each event? Use `AGE(CURRENT_DATE, event_date)` from the perspective of '2024-03-15'.

### 8. Formatting

Show events with date formatted as 'Day, Month DD, YYYY'.

## Expected Output for Task 4

```
      month       | count
-------------------+-------
 2024-01-01 00:00:00|     3
 2024-02-01 00:00:00|     2
 2024-03-01 00:00:00|     2
```

## Hints

- `EXTRACT(DOW FROM date)` — 0 is Sunday, 5 is Friday, 6 is Saturday
- `DATE_TRUNC('month', event_date)` for grouping
- `event_time + (duration_minutes || ' minutes')::INTERVAL` for end time
- `TO_CHAR(event_date, 'Day, Month DD, YYYY')` for formatting
