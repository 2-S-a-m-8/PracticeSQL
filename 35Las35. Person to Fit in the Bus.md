# 35. Last Person to Fit in the Bus

## Problem Description

**Table:** `Queue`

| Column Name | Type    |
|-------------|---------|
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |

- Each row describes a person in a queue waiting to board a bus.
- The column `turn` determines boarding order. `turn = 1` is first, `turn = n` is last.
- Each person has a weight.
- The bus has a total **weight limit of 1000 kg**.
- Only one person boards per turn.

Write a query to find the `person_name` of the **last person who can fit on the bus** without the total weight exceeding 1000 kg.

---

## Example

**Input:**

`Queue` table:

| person_id | person_name | weight | turn |
|-----------|-------------|--------|------|
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |

**Output:**

| person_name |
|-------------|
| John Cena   |

---

## SQL Query

```sql
WITH cte AS (
  SELECT *,
         SUM(weight) OVER (ORDER BY turn) AS cu_sum
  FROM Queue
)
SELECT person_name
FROM cte
WHERE cu_sum <= 1000
ORDER BY turn DESC
LIMIT 1;
```
---
## Explanation
1. CTE (cte): Calculates the cumulative weight (cu_sum) using SUM(...) OVER (ORDER BY turn).
2. Filter: Keep only rows where cu_sum ≤ 1000.
3. ORDER BY turn DESC LIMIT 1: Pick the last person (largest turn value) among those who fit.
---
