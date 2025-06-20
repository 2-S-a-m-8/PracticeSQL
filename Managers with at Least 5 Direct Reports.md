# 13. Managers with at Least 5 Direct Reports

**Difficulty**: Medium  
**Tags**: `JOIN`, `GROUP BY`, `Subquery`, `Self-Join`  
**Topic**: SQL  

---

## Problem Description

You are given the following table:

### `Employee` Table

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |

- `id` is the primary key.
- `managerId` is a reference to `id` of another employee (nullable).
- No employee will be the manager of themselves.

---

## Objective

Find the names of employees who are **managers** and have at least **five direct reports**.

Return the result table in any order.

---

## Explanation of Approach

1. **Group employees by `managerId`** — Each group represents all employees who report to the same manager.
2. Use `HAVING COUNT(*) >= 5` to filter only those groups (i.e., managers) that have at least 5 direct reports.
3. From this filtered list of manager IDs, **find their names** in the `Employee` table using a subquery.

This approach avoids explicit self-joins by leveraging a subquery to filter by `managerId`.

---

## SQL Query

```sql
SELECT name 
FROM Employee 
WHERE id IN (
    SELECT managerId 
    FROM Employee 
    GROUP BY managerId 
    HAVING COUNT(*) >= 5
);
```
