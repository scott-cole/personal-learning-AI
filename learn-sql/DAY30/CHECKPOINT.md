# DAY30 — Checkpoint

## Questions

**1. Why does the `likes` table need a UNIQUE constraint on (user_id, post_id)?**

<details>
<summary>Answer</summary>
To prevent a user from liking the same post more than once. Each (user, post) pair should be unique — that's the business rule.
</details>

**2. How would you find mutual followers (users who follow each other)?**

<details>
<summary>Answer</summary>
```sql
SELECT f1.follower_id, f1.followee_id
FROM follows f1
JOIN follows f2 ON f1.follower_id = f2.followee_id
               AND f1.followee_id = f2.follower_id
WHERE f1.follower_id < f1.followee_id;
```
</details>

**3. Write a query to find the top 3 most-followed users using a window function.**

<details>
<summary>Answer</summary>
```sql
SELECT * FROM (
    SELECT followee_id, COUNT(*) AS followers,
        RANK() OVER (ORDER BY COUNT(*) DESC) AS rnk
    FROM follows GROUP BY followee_id
) ranked WHERE rnk <= 3;
```
</details>

**4. How would you implement a "follow suggestion" feature (friends-of-friends) in SQL?**

<details>
<summary>Answer</summary>
```sql
SELECT DISTINCT u.* FROM users u
JOIN follows f2 ON u.id = f2.followee_id
JOIN follows f1 ON f1.followee_id = f2.follower_id
WHERE f1.follower_id = 1           -- The target user
  AND u.id <> 1                     -- Not themselves
  AND u.id NOT IN (                 -- Not already following
    SELECT followee_id FROM follows WHERE follower_id = 1
  );
```
</details>

**5. What indexes would you create for a social media database to optimize common queries?**

<details>
<summary>Answer</summary>
- `likes(post_id)` — to count likes per post quickly
- `likes(user_id, post_id)` — for the unique constraint and "has user liked this post?"
- `posts(user_id, created_at)` — for a user's timeline
- `follows(follower_id)` — who does a user follow
- `follows(followee_id)` — who follows a user
- `comments(post_id)` — comments on a post
- `posts(created_at DESC)` — for the global feed
</details>
