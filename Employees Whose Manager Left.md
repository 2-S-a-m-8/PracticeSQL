# 37. Employees Whose Manager Left the Company

**Difficulty:** Easy  
**Tags:** Subquery, Filtering, NOT IN, NULL handling  

---

## Problem Description

### Table: `Employees`

| Column Name | Type    |
|-------------|---------|
| employee_id | int     |
| name        | varchar |
| manager_id  | int     |
| salary      | int     |

- `employee_id` is the **primary key**.
- Each row contains an employee's name, salary, and their `manager_id`.
- Some employees may not have a manager (`manager_id` is NULL).
- If a manager leaves the company, **their row is deleted** from this table, but their subordinates **still retain the old `manager_id`**.

---

## Objective

Return the IDs of employees who:

1. Have **salary strictly less than 30000**
2. And whose **manager has left the company** (i.e., their `manager_id` is not present in any `employee_id` in the table)

Return the result **ordered by `employee_id`**.

---

## Example

### Input: `Employees` table

| employee_id | name      | manager_id | salary |
|-------------|-----------|------------|--------|
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | NULL       | 31000  |
| 13          | Emery     | NULL       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | NULL       | 50937  |
| 11          | Joziah    | 6          | 28485  |

### Output

| employee_id |
|-------------|
| 11          |

---

## SQL Query

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
  AND manager_id NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```
---
## Explanation
- salary < 30000: filters for low-salary employees
- manager_id NOT IN (...): filters those whose manager_id is not present in the list of existing employee_ids (i.e., their manager has left the company)
- ORDER BY employee_id: sorts the result by employee ID as required
- NOT IN automatically excludes NULLs, so manager_id IS NULL rows are safely ignored.
- We don’t need to write manager_id IS NOT NULL because NULL values are inherently excluded from NOT IN.
---
