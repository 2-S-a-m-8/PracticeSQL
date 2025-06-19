### 11. Employee Bonus  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Tags:** SQL, Joins, NULL Handling, Filtering  

---

### Problem Statement

You are given two tables: `Employee` and `Bonus`.

**Table: Employee**

| Column Name | Type    |
|-------------|---------|
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |

- `empId` is the primary key (unique identifier).
- Each row contains the name of an employee, their salary, and the ID of their supervisor.

**Table: Bonus**

| Column Name | Type |
|-------------|------|
| empId       | int  |
| bonus       | int  |

- `empId` is a foreign key referencing the `Employee` table.
- Each row represents the bonus amount for a specific employee.
- It is possible for some employees not to have any entry in the `Bonus` table.

**Task:**  
Write a SQL query to return the `name` and `bonus` of all employees whose bonus is **less than 1000** or **who did not receive a bonus** (i.e., `NULL`).  
The result can be returned in any order.

---

### Sample Input

**Employee Table**

| empId | name   | supervisor | salary |
|-------|--------|------------|--------|
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |

**Bonus Table**

| empId | bonus |
|-------|-------|
| 2     | 500   |
| 4     | 2000  |

---

### Expected Output

| name  | bonus |
|-------|-------|
| Brad  | null  |
| John  | null  |
| Dan   | 500   |

---

### Explanation
1. SELECT a.name, b.bonus
We select the employee's name from the Employee table (a.name)
We also select the bonus amount, which may be NULL if no bonus record exists.

2. FROM Employee a
We start with the Employee table, giving it an alias a for easier reference.

3. LEFT JOIN Bonus b ON a.empId = b.empId
This is a left outer join, meaning:
All employees will be included in the result.
If an employee has a matching row in the Bonus table (i.e., same empId), their bonus will be retrieved.
If no match is found in the Bonus table, their bonus will be NULL.

4. WHERE b.bonus < 1000 OR b.bonus IS NULL
This WHERE clause filters the results:
Keep rows where the bonus is less than 1000.
Also keep rows where there is no bonus record (b.bonus IS NULL).

5. Note: Without the b.bonus IS NULL condition, employees without bonuses would be excluded from the result, even with a LEFT JOIN.



---

### Final SQL Query

```sql
SELECT a.name, b.bonus
FROM Employee a
LEFT JOIN Bonus b ON a.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;
```
