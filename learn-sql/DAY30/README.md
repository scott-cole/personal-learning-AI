# DAY30 — Final Project: Social Media Database

## Capstone Project

You've completed 29 days of SQL. You've learned SELECT, WHERE, JOINs, aggregation, subqueries, set operations, string/date functions, CASE, indexes, transactions, constraints, views, normalization, window functions, CTEs, and query planning.

Now you'll build a complete social media database — the kind of schema that powers platforms like Instagram, Twitter, or TikTok. This project ties together everything you've learned.

## Scenario

Design and implement a database for a social media platform. Users can:
- Create profiles
- Write posts
- Like posts
- Follow other users
- Comment on posts

## Schema Design

Design a normalized schema (3NF) with these entities:

### users

| Column | Type | Notes |
|--------|------|-------|
| id | SERIAL PK | |
| username | VARCHAR(50) | UNIQUE, NOT NULL |
| email | VARCHAR(200) | UNIQUE, NOT NULL |
| display_name | VARCHAR(100) | |
| bio | TEXT | |
| created_at | TIMESTAMPTZ | DEFAULT NOW() |

### posts

| Column | Type | Notes |
|--------|------|-------|
| id | SERIAL PK | |
| user_id | INTEGER | FK → users(id) |
| content | TEXT | NOT NULL |
| created_at | TIMESTAMPTZ | DEFAULT NOW() |

### likes

| Column | Type | Notes |
|--------|------|-------|
| id | SERIAL PK | |
| user_id | INTEGER | FK → users(id) |
| post_id | INTEGER | FK → posts(id) |
| created_at | TIMESTAMPTZ | DEFAULT NOW() |
| UNIQUE | (user_id, post_id) | One like per user per post |

### follows

| Column | Type | Notes |
|--------|------|-------|
| id | SERIAL PK | |
| follower_id | INTEGER | FK → users(id) |
| followee_id | INTEGER | FK → users(id) |
| created_at | TIMESTAMPTZ | DEFAULT NOW() |
| UNIQUE | (follower_id, followee_id) | No duplicate follows |
| CHECK | follower_id <> followee_id | Can't follow yourself |

### comments

| Column | Type | Notes |
|--------|------|-------|
| id | SERIAL PK | |
| user_id | INTEGER | FK → users(id) |
| post_id | INTEGER | FK → posts(id) |
| content | TEXT | NOT NULL |
| created_at | TIMESTAMPTZ | DEFAULT NOW() |

## Sample Data

Insert:
- 8 users with realistic usernames
- 15-20 posts spread across users (some users more active than others)
- 30-40 likes spreading across posts
- 15 follow relationships (not everyone follows everyone)
- 20-25 comments on various posts

## Queries

Write these queries. Each should be efficient and use appropriate SQL features.

### Basic queries
1. Show the 10 most recent posts with the author's username and display name
2. Show the 5 most-liked posts, with the like count and author's username
3. Find all users who a specific user (e.g., user_id = 1) is following, with their latest post date

### Intermediate queries
4. For each user, show their total posts, total likes received (across all their posts), and total comments received
5. Find users who have never posted (using NOT EXISTS or LEFT JOIN)
6. Show the follow suggestions for a user: users followed by people the user follows, but not yet followed by the user. (Friends-of-friends.)

### Advanced queries using window functions
7. Rank users by total likes received (ROW_NUMBER, RANK, DENSE_RANK — show all three)
8. For each user, show their most recent 3 posts with row numbers per user (PARTITION BY)
9. Running total of posts per day across the entire platform

### Complex queries with CTEs
10. Use a recursive CTE to show the follow chain starting from a user: "user follows X, who follows Y, who follows Z"
11. Use a CTE to find the most active hour of the day for posting (across all users)

### Performance and maintenance
12. Create appropriate indexes for the queries above (consider what columns are filtered/joined)
13. Write a transaction that safely transfers a follow from one user to another (unfollow + refollow atomically)
14. Create a view `user_summary` that shows each user's stats (post count, like count, follower count, following count)
15. Use EXPLAIN ANALYZE on query 2 (top 5 liked posts) and optimize with indexes

### Challenge queries
16. Find the "mutual followers" — pairs of users who follow each other
17. Find users who have liked every post by a specific user (correlated subquery with NOT EXISTS)
18. Calculate the "influence score" for each user: followers × avg likes per post × posting frequency

## Deliverable

Create `social_media_project.sql` containing:
1. All CREATE TABLE statements (with constraints and FKs)
2. All INSERT statements for sample data
3. All 18+ queries, each clearly labelled with a comment

The file should run end-to-end without errors using `\i social_media_project.sql`.

## Success Criteria

- Schema is in 3NF
- All foreign keys are properly defined
- Indexes exist for the most common query patterns
- Queries run efficiently (check with EXPLAIN ANALYZE)
- Recursive CTE works for follow chains
- Window functions are used correctly
- The transaction is atomic and safe

Good luck! This is your capstone — make it count.
