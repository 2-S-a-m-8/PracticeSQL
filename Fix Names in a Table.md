# 44. Fix Names in a Table

**Difficulty**: Easy  
**Topic**: String Manipulation  
**Tags**: `UPPER`, `LOWER`, `LEFT`, `RIGHT`, `CONCAT`, `ORDER BY`  
**Platform**: LeetCode  
**Tables**: `Users`

---

## Problem Statement

You are given a table called `Users` with the following schema:

### Users Table

| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| name        | varchar |

- `user_id` is the **primary key**.
- `name` contains names with **random casing** (only alphabets, lowercase and uppercase).

---

## Task

Write a query to fix the `name` column such that:
- Only the **first character is uppercase**
- The rest of the characters are **lowercase**

Return the table ordered by `user_id`.

---

## Example

### Input

| user_id | name  |
|---------|-------|
| 1       | aLice |
| 2       | bOB   |

### Output

| user_id | name  |
|---------|-------|
| 1       | Alice |
| 2       | Bob   |

---

## SQL Query

```sql
SELECT 
    user_id, 
    CONCAT(
        UPPER(LEFT(name, 1)), 
        LOWER(RIGHT(name, LENGTH(name) - 1))
    ) AS name 
FROM Users 
ORDER BY user_id;
```
## Explanation
1. LEFT(name, 1) → Extracts the first character of the name.
2. UPPER(...) → Converts it to uppercase.
3. RIGHT(name, LENGTH(name) - 1) → Gets the rest of the characters (from second character to end).
4. LOWER(...) → Converts the rest to lowercase.
5. CONCAT(...) → Joins the capitalized first character and the lowercase remainder.
6. ORDER BY user_id → Ensures the result is sorted by user ID.
---
