# 30. The Number of Employees Which Report to Each Employee

## Problem Description

Given the `Employees` table:

| Column Name  | Type    |
|--------------|---------|
| employee_id  | int     |
| name         | varchar |
| reports_to   | int     |
| age          | int     |

- `employee_id` is the unique primary key.
- `reports_to` refers to the `employee_id` of the manager.
- An employee may not report to anyone (i.e., `reports_to` is `NULL`).

### Goal

Find all employees who are **managers** (i.e., have at least one employee reporting to them), and return:
- `employee_id`
- `name`
- Number of direct reports as `reports_count`
- Average age of direct reports as `average_age` (rounded to the nearest integer)

### Output Format

Return a table like:

| employee_id | name    | reports_count | average_age |
|-------------|---------|----------------|-------------|
| 1           | Michael | 2              | 40          |

Order the results by `employee_id`.

---

## Sample Input

```sql
Employees table:
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
```
## Sample Output

```sql
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
```

## SQL Query

```sql
SELECT 
    e.employee_id,
    e.name,
    COUNT(e1.employee_id) AS reports_count,
    ROUND(AVG(e1.age)) AS average_age
FROM Employees e
JOIN Employees e1 
    ON e.employee_id = e1.reports_to
GROUP BY e.employee_id
ORDER BY e.employee_id;
```

## Explanation
1. This query performs a self-join:
   a. e refers to the manager.
   b. e1 refers to their direct reports (employees whose reports_to = e.employee_id).
2. ```COUNT(e1.employee_id)``` counts how many people report to each manager.
3. ```AVG(e1.age)``` computes the average age of those employees, which we then ROUND() to the nearest integer.
4. We use GROUP BY on e.employee_id to aggregate each manager's direct reports.
5. The final result is sorted by manager ID.
