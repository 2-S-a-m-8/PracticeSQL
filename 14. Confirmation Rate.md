# 14: Confirmation Rate

**Difficulty**: Medium  
**Tags**: `JOIN`, `GROUP BY`,`CASE`  
**Topic**: SQL  
## Problem Description

We are given two tables:

### Signups
| Column Name | Type     |
|-------------|----------|
| user_id     | int      |
| time_stamp  | datetime |

- `user_id` is unique.
- This table tracks when each user signed up.

### Confirmations
| Column Name | Type     |
|-------------|----------|
| user_id     | int      |
| time_stamp  | datetime |
| action      | ENUM('confirmed', 'timeout') |

- (user_id, time_stamp) together form the primary key.
- `user_id` is a foreign key referencing `Signups`.
- `action` represents the result of a confirmation request. A user can either confirm (`'confirmed'`) or let the request expire (`'timeout'`).

### Goal

Calculate the confirmation rate for each user:
- A user’s **confirmation rate** is the number of `'confirmed'` actions divided by their total number of confirmation requests.
- If a user made no confirmation requests, their rate is 0.00.
- The final result must show each `user_id` and their `confirmation_rate`, rounded to **two decimal places**.

---

## Sample Input

### Signups

| user_id | time_stamp          |
|---------|---------------------|
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |

### Confirmations

| user_id | time_stamp          | action    |
|---------|---------------------|-----------|
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |

### Expected Output

| user_id | confirmation_rate |
|---------|-------------------|
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |

---

## SQL Query

```sql
SELECT 
    a.user_id, 
    COALESCE(
        ROUND(
            SUM(CASE WHEN b.action = 'confirmed' THEN 1 ELSE 0 END) * 1.0 
            / COUNT(b.action),
        2),
    0) AS confirmation_rate
FROM 
    Signups a
LEFT JOIN 
    Confirmations b 
ON 
    a.user_id = b.user_id
GROUP BY 
    a.user_id;
```
## Explanation

1. Left Join Signups and Confirmations
```
FROM Signups a
LEFT JOIN Confirmations b ON a.user_id = b.user_id
```
We use a LEFT JOIN to ensure all users from the Signups table are included in the result.
This includes users who have no confirmation attempts. For those users, the joined b columns (especially action) will be NULL.

2. Count Confirmed Requests Using CASE WHEN
```
SUM(CASE WHEN b.action = 'confirmed' THEN 1 ELSE 0 END)
```
This counts how many confirmation attempts resulted in 'confirmed' for each user.
If a user made no requests, this sum becomes NULL, which is fine because we handle it later with COALESCE.

3. Count Total Requests
```
COUNT(b.action)
```
COUNT(column) skips NULLs. So for users with no confirmations, this count becomes 0.
For other users, it returns the total number of confirmation actions (both 'confirmed' and 'timeout').

4. Compute Rate and Force Decimal Division

```
COALESCE(
        ROUND(
            SUM(CASE WHEN b.action = 'confirmed' THEN 1 ELSE 0 END) * 1.0 
            / COUNT(b.action),
        2),
    0)
```
Multiplying by 1.0 ensures the division is in floating-point and not integer.
For example, 1 / 2 = 0.5 and not 0.
For users who made no confirmation attempts, both the numerator and denominator are NULL.
To avoid returning NULL, we wrap the full expression in COALESCE(..., 0) to return 0.00.

