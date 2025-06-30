# 33. Consecutive Numbers

## Problem Statement

You are given a table called `Logs` with the following schema:

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| num         | varchar |

- `id` is the **primary key** and auto-increments starting from 1.
- Each row contains a number that may or may not repeat.

### Task

Find all numbers that appear **at least three times consecutively**.

Return the result in a column named `ConsecutiveNums`. The output order does not matter.

---

## Example

### Input

Logs table:
```sql
+----+-----+
| id | num |
+----+-----+
| 1 | 1 |
| 2 | 1 |
| 3 | 1 |
| 4 | 2 |
| 5 | 1 |
| 6 | 2 |
| 7 | 2 |
+----+-----+
```

### Output
```sql
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1 |
+-----------------+
```

### Explanation

- The number `1` appears **three times in a row** at `id` = 1, 2, 3 → so it is included.
- The number `2` appears consecutively **only twice** → not enough to qualify.

---

## SQL Query 

```sql
SELECT DISTINCT a.num AS ConsecutiveNums
FROM Logs a, Logs b, Logs c
WHERE a.id = b.id - 1
  AND b.id = c.id - 1
  AND a.num = b.num
  AND b.num = c.num;
```
---
# Explanation
1. This query joins the Logs table to itself three times (a, b, c) to examine windows of three consecutive entries.
2. The conditions a.id = b.id - 1 and b.id = c.id - 1 ensure the rows are adjacent by ID.
3. Then it checks if the num values in all three rows are equal.
4. If true, that number is considered to appear three times consecutively.
5. DISTINCT ensures the output contains unique values only.

---

