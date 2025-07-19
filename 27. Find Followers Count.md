# 27. Find Followers Count

**Difficulty:** Easy  
**Tags:** Aggregation, Grouping

---

## Problem Description

You are given a table `Followers` representing a social media relationship between users. Each row contains a `user_id` and a `follower_id`, indicating that `follower_id` follows `user_id`.

### Table: `Followers`

| Column Name | Type |
|-------------|------|
| user_id     | int  |
| follower_id | int  |

- The combination `(user_id, follower_id)` is the **primary key**.
- This table represents a directed edge in a social network where one user follows another.

---

## Objective

For each user, return the **number of unique followers** they have.

Return the result table **ordered by `user_id` in ascending order**.

---

## Example Input

**Followers Table:**

| user_id | follower_id |
|---------|-------------|
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |

---

## Expected Output

| user_id | followers_count |
|---------|-----------------|
| 0       | 1               |
| 1       | 1               |
| 2       | 2               |

---

## SQL Query

```sql
SELECT user_id, 
       COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id;
```
## Explanation
1. ```GROUP BY user_id```
→ Groups all followers for each user.

2. ```COUNT(DISTINCT follower_id)```
→ Counts the number of unique followers per user.

3. ```ORDER BY user_id```
→ Ensures output is sorted by user_id as required.
