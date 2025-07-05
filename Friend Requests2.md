# 41. Who Has the Most Friends

**Difficulty:** Medium  
**Topic:** Aggregation, UNION  
**Tags:** SQL, Friends Count, Leaderboard

---

## Table Schema

**RequestAccepted**

| Column Name   | Type | Description                                           |
|---------------|------|-------------------------------------------------------|
| requester_id  | int  | ID of the user who sent the friend request            |
| accepter_id   | int  | ID of the user who accepted the friend request        |
| accept_date   | date | Date when the friend request was accepted             |

Primary Key: `(requester_id, accepter_id)`

---

## Problem Statement

Write a SQL query to find:

- The user who has the **most total friends**.
- The total number of friends that user has.

Each row in the `RequestAccepted` table represents a confirmed friendship. Friendship is mutual — if A sends a request to B and B accepts, both A and B are considered friends.

The output should return **only one user** (it's guaranteed there's only one such person in test cases).

---

## Example Input

**RequestAccepted Table:**

| requester_id | accepter_id | accept_date |
|--------------|-------------|-------------|
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |

---

## Expected Output

| id | num |
|----|-----|
| 3  | 3   |

**Explanation:**  
- User `3` is friends with `1`, `2`, and `4` — total **3 friends**.
- This is more than any other user.

---

## Approach

1. Friendships are **mutual**, so both `requester_id` and `accepter_id` count as a friend.
2. Use `UNION ALL` to combine both columns into a single column of `id`.
3. Group by `id` and count how many times they appear — each appearance = 1 friendship.
4. Sort by count in descending order and return only the top row.

---

## SQL Query

```sql
SELECT id, COUNT(*) AS num
FROM (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
) AS temp
GROUP BY id
ORDER BY COUNT(*) DESC
LIMIT 1;
```
---
