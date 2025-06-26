# 24. User Activity for the Past 30 Days I

**Difficulty:** Easy  
**Tags:** Grouping, Aggregation, Filtering

---

## Problem Statement

You are given a table `Activity`:

| Column Name   | Type    |
|---------------|---------|
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |

- Each row in the table represents an activity of a user on a social media platform.
- Each `session_id` belongs to exactly one `user_id`.
- The `activity_type` is one of: `'open_session'`, `'end_session'`, `'scroll_down'`, `'send_message'`.
- This table may contain duplicate rows.

---

## Objective

Write a SQL query to **find the daily active user count** for the 30-day period ending **2019-07-27** (inclusive).  
A user is considered active on a day if they performed **at least one activity** that day.  
Return the result with the columns:

- `day`: the date
- `active_users`: number of unique users active on that day

Only include days with **at least one active user**.

---

## Example Input

**Activity Table:**

| user_id | session_id | activity_date | activity_type |
|---------|------------|---------------|---------------|
| 1       | 1          | 2019-07-20    | open_session  |
| 1       | 1          | 2019-07-20    | scroll_down   |
| 1       | 1          | 2019-07-20    | end_session   |
| 2       | 4          | 2019-07-20    | open_session  |
| 2       | 4          | 2019-07-21    | send_message  |
| 2       | 4          | 2019-07-21    | end_session   |
| 3       | 2          | 2019-07-21    | open_session  |
| 3       | 2          | 2019-07-21    | send_message  |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |

---

## Expected Output

| day        | active_users |
|------------|--------------|
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |

---

## SQL Solution

```sql
SELECT 
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date > '2019-06-27' AND activity_date <= '2019-07-27'
GROUP BY activity_date;
```
## Explanation

1. ```SELECT activity_date AS day```
→ We want to display the date of activity as day.

2. ```COUNT(DISTINCT user_id)```
→ For each day, we count the number of unique users who performed at least one activity.

3. ```FROM Activity```
→ We are selecting from the provided Activity table.

4. ```WHERE activity_date > '2019-06-27' AND activity_date <= '2019-07-27'```
→ We limit the results to a 30-day window ending on 2019-07-27.

5. ```GROUP BY activity_date```
→ This ensures we get one row per date with the count of distinct active users.
```
