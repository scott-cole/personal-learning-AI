# DAY30 — Drill: Social Media Final Project

## Task

Build a complete social media database in `social_media_project.sql`. This is your capstone — all 29 previous days lead to this.

## Setup Instructions

Create a database:

```sql
CREATE DATABASE social_media;
\c social_media;
```

Run your project file:

```sql
\i social_media_project.sql
```

## Required Tables

Create users, posts, likes, follows, and comments with proper constraints, types, and foreign keys. (See README.md for full schema.)

## Sample Data Targets

| Table | Minimum Rows |
|-------|-------------|
| users | 8 |
| posts | 15 |
| likes | 30 |
| follows | 15 |
| comments | 20 |

## Query Checklist

### Basic (1-3)
- [ ] 1. Recent 10 posts with author info
- [ ] 2. Top 5 liked posts with counts
- [ ] 3. Users followed by user X with latest post date

### Intermediate (4-6)
- [ ] 4. User stats (posts, likes received, comments received)
- [ ] 5. Users who never posted
- [ ] 6. Friend-of-friend suggestions

### Window Functions (7-9)
- [ ] 7. User ranking by likes (ROW_NUMBER, RANK, DENSE_RANK)
- [ ] 8. Each user's 3 most recent posts
- [ ] 9. Running daily post total

### CTEs (10-11)
- [ ] 10. Recursive follow chain
- [ ] 11. Most active posting hour

### Performance (12-15)
- [ ] 12. Create appropriate indexes
- [ ] 13. Atomic follow transfer transaction
- [ ] 14. user_summary view
- [ ] 15. EXPLAIN ANALYZE and optimize query 2

### Challenge (16-18)
- [ ] 16. Mutual followers
- [ ] 17. Users who liked ALL posts by a specific user
- [ ] 18. Influence score formula

## Hints

- Create tables in dependency order (users → posts → likes/follows → comments)
- Insert users first, then posts, then likes/follows, then comments
- Use `RETURNING *` to verify inserts
- For query 6 (friend-of-friend): JOIN follows twice
- For query 10 (recursive follow chain): start with a specific user, follow their followees
- For query 17 (liked ALL posts): use `NOT EXISTS` with a subquery finding posts by user X that the outer user hasn't liked
- Influence score: `follower_count * avg_likes_per_post * posts_per_week`
- Test each query individually before assembling the final file
