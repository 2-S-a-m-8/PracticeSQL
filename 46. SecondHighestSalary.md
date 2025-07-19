
# 46. Second Highest Salary

## Problem Statement

You are given a table named `Employee`:

```
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
```

- `id` is the primary key.
- Each row contains an employee's salary.

### Goal:
Write a SQL query to **return the second highest distinct salary** from the `Employee` table. If there is no such salary, return `null`.

---

## Example Inputs and Expected Outputs

### Example 1:
**Input:**
```text
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

**Output:**
```text
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

---

### Example 2:
**Input:**
```text
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
```

**Output:**
```text
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
```

---

## Solution Explanation

To find the second highest **distinct** salary, we use a Common Table Expression (CTE) along with the `DENSE_RANK()` window function:

### Key Steps:
1. **CTE Creation**: The CTE computes a ranking of all distinct salaries in descending order.
2. **DENSE_RANK**: This function assigns a rank to each unique salary, with no gaps in ranking.
   - e.g., for salaries [300, 200, 200, 100], `DENSE_RANK` assigns ranks as [1, 2, 2, 3].
3. **Filtering**: We select the salary where the rank is 2 (i.e., the second highest).
4. **Null Handling**: If no second highest exists, the subquery returns null.

---

## SQL Query Solution

```sql
WITH cte AS (
    SELECT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM Employee
)
SELECT (
    SELECT salary
    FROM cte
    WHERE rnk = 2
    LIMIT 1
) AS SecondHighestSalary;
```

---


## Notes

- `DENSE_RANK()` avoids skipping rank values even when multiple employees share the same salary.
- The outer `SELECT` ensures a single-row result, returning `null` if rank 2 doesn't exist.
- This solution is compatible with most SQL engines like MySQL and PostgreSQL.

---
