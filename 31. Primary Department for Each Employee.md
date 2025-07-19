# 31. Primary Department for Each Employee

## Problem Description

Given a table `Employee` with the following structure:

| Column Name     | Type    |
|------------------|---------|
| employee_id      | int     |
| department_id    | int     |
| primary_flag     | varchar |

- Each employee may belong to **one or more departments**.
- If an employee belongs to **only one department**, that is their **primary department**.
- If they belong to **more than one**, the row with `primary_flag = 'Y'` marks the **primary department**.

---

## Objective

Return the `employee_id` and `department_id` of each employee's **primary department**.

### Output Format

| employee_id | department_id |
|-------------|----------------|
| 1           | 1              |
| 2           | 1              |
| 3           | 3              |
| 4           | 3              |

---

## Sample Input

```sql
Employee =
+-------------+----------------+---------------+
| employee_id | department_id  | primary_flag  |
+-------------+----------------+---------------+
| 1           | 1              | N             |
| 2           | 1              | Y             |
| 2           | 2              | N             |
| 3           | 3              | N             |
| 4           | 2              | N             |
| 4           | 3              | Y             |
```
---

## Sample output
```sql
+-------------+----------------+
| employee_id | department_id  |
+-------------+----------------+
| 1           | 1              |
| 2           | 1              |
| 3           | 3              |
| 4           | 3              |
```

## SQL Query

```sql
SELECT employee_id, department_id
FROM Employee
GROUP BY employee_id
HAVING COUNT(department_id) = 1

UNION ALL

SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y';
```
---

## Explanation
1. The first ```SELECT``` handles employees with only one department using ```GROUP BY``` and ```HAVING COUNT(department_id) = 1```.
2. The second ```SELECT``` handles employees with multiple departments, selecting only those with primary_flag = 'Y'.
3. ```UNION ALL``` is used to combine both result sets without removing duplicates (since employee_id is unique in each group).
