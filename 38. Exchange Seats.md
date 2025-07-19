# 38. Exchange Seats

##  Difficulty: Medium  
**Topic:** Window Functions, Conditional Logic  
**Company Tags:** Bloomberg  

---

## Table: `Seat`

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| student     | varchar |

- `id` is the primary key for this table.
- Each row of this table indicates the name and the ID of a student.
- The `id` sequence always starts from 1 and increments continuously.

---

## Problem Statement

Write a SQL query to **swap the seat id of every two consecutive students**.  
If the number of students is odd, the student with the last id remains in place.

Return the result table **ordered by id in ascending order**.

---

## Input Example

```text
Seat table:
+----+---------+
| id | student |
+----+---------+
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |
+----+---------+
```
---
## Output Example

```text
+----+---------+
| id | student |
+----+---------+
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |
+----+---------+
```
---
## SQL Query
```sql
SELECT 
    id,
    CASE
        WHEN id % 2 = 0 THEN LAG(student) OVER (ORDER BY id)
        ELSE COALESCE(LEAD(student) OVER (ORDER BY id), student)
    END AS student
FROM Seat
ORDER BY id;
```
---
## Explanation
1. LAG(student) gets the previous row's student — used for even ids (they take the student of the previous odd id)
2. LEAD(student) gets the next row's student — used for odd ids (they take the student of the next even id)
3. COALESCE(..., student) ensures that the last odd student (if no partner exists) keeps their own name
4. ORDER BY id ensures that final output respects the original seat ID order
---
