# 18. Percentage of Users Attended a Contest
**Difficulty**: Easy  
**Tags**: `JOIN`, `GROUP BY`, `AGGREGATE`, `ROUND`  

## Problem Statement

You are given two tables:

### Table: Users

| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| user_name   | varchar |

- `user_id` is the **primary key** for this table.
- Each row contains the name and ID of a user.

### Table: Register

| Column Name | Type |
|-------------|------|
| contest_id  | int  |
| user_id     | int  |

- (`contest_id`, `user_id`) is the **primary key**.
- Each row represents a user's registration in a contest.

---

## Task

Write a query to return the percentage of **unique users** who registered for each contest, **rounded to 2 decimal places**. Return the results ordered by `percentage DESC`, and in case of a tie, by `contest_id ASC`.

---

## 📊 Example Input

### Users

| user_id | user_name |
|---------|-----------|
| 6       | Alice     |
| 2       | Bob       |
| 7       | Alex      |

### Register

| contest_id | user_id |
|------------|---------|
| 215        | 6       |
| 209        | 2       |
| 208        | 2       |
| 210        | 6       |
| 208        | 6       |
| 209        | 7       |
| 209        | 6       |
| 215        | 7       |
| 208        | 7       |
| 210        | 2       |
| 207        | 2       |
| 210        | 7       |

---

## Output

| contest_id | percentage |
|------------|------------|
| 208        | 100.00     |
| 209        | 100.00     |
| 210        | 100.00     |
| 215        | 66.67      |
| 207        | 33.33      |

---


## 💡 SQL Solution

```sql
SELECT 
    contest_id,
    ROUND(COUNT(DISTINCT r.user_id) * 100.0 / (SELECT COUNT(DISTINCT user_id) FROM Users), 2) AS percentage
FROM 
    Register r
GROUP BY 
    contest_id
ORDER BY 
    percentage DESC, contest_id;

```

## Explanation
1. FROM Register r<br>
We're using the Register table (aliased as r) as the base table. This table contains all records of which user registered for which contest.

2. GROUP BY contest_id<br>
We want to calculate percentages per contest, so we group all entries in the Register table by contest_id.
For example:
contest_id = 208
users = [2, 6, 7]

4. COUNT(DISTINCT r.user_id)<br>
For each contest, we count how many distinct users registered. We use DISTINCT in case the same user registered more than once (even though the schema says the pair is unique, it's good practice for clarity).
This gives us the number of users who registered for each contest.

5. (SELECT COUNT(DISTINCT user_id) FROM Users)<br>
This is a scalar subquery that computes the total number of unique users in the system.
This is the denominator for our percentage calculation:
total_users = 3  (Alice, Bob, Alex)

6. ROUND(..., 2)<br>
We multiply the ratio by 100 to get a percentage, and then round to 2 decimal places, as the output format requires.
So the full expression:
ROUND(COUNT(DISTINCT r.user_id) * 100.0 / (SELECT COUNT(DISTINCT user_id) FROM Users), 2)
might become:
ROUND(3 * 100.0 / 3, 2) = 100.00

7. ORDER BY percentage DESC, contest_id ASC
To satisfy the output requirement:
First, sort contests in descending order of percentage (i.e., highest participation first).
Then, for contests with the same percentage, sort by contest ID in ascending order.



---
