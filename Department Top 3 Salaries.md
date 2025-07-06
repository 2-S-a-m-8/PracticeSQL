# 43. Department Top Three Salaries

**LeetCode Problem ID**: 185  
**Difficulty**: Hard  
**Topic**: Window Functions, Ranking  
**Tags**: `DENSE_RANK`, `JOIN`, `Window Function`, `Filtering`  
**Tables**: `Employee`, `Department`

---

## Problem Statement

You are given two tables:
- `Employee(id, name, salary, departmentId)`
- `Department(id, name)`

Each employee belongs to one department. You are asked to **find the employees who are high earners in each department**, i.e., the **top three unique salaries** in each department.

Return the result with columns:  
- `Department` (department name)  
- `Employee` (employee name)  
- `Salary` (employee salary)

> If multiple employees share the same salary, include all of them.

---

## Sample Input

### Employee Table

| id | name  | salary | departmentId |
|----|-------|--------|--------------|
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |

### Department Table

| id | name  |
|----|-------|
| 1  | IT    |
| 2  | Sales |

---

## Expected Output

| Department | Employee | Salary |
|------------|----------|--------|
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |

---

## SQL Query Solution

```sql
WITH RankedSalaries AS (
    SELECT 
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        DENSE_RANK() OVER (
            PARTITION BY d.name 
            ORDER BY e.salary DESC
        ) AS rnk
    FROM Employee e
    JOIN Department d 
      ON e.departmentId = d.id
)
SELECT 
    Department,
    Employee,
    Salary
FROM RankedSalaries
WHERE rnk <= 3;
```
## Explanation
1: Join<br>
We first join the Employee and Department tables to get department names with employee data.<br>
2: Rank Salaries<br>
Using the DENSE_RANK() window function, we rank salaries within each department, in descending order. This gives the highest salaries rank 1, the next unique salary rank 2, and so on.<br>
3: Filter Top 3<br>
We filter only those rows where rnk <= 3, meaning we are selecting only the top three unique salary levels per department.<br>

``Why DENSE_RANK?
We use DENSE_RANK() instead of ROW_NUMBER() or RANK() to ensure we don’t skip ranks when multiple employees share the same salary.``<br>
For example:<br>
Salaries: 90000, 85000, 85000, 70000<br>
DENSE_RANK gives ranks: 1, 2, 2, 3 <br>
ROW_NUMBER would wrongly skip someone with same salary.<br>

---
