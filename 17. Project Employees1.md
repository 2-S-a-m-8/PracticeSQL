# 17. Project Employees I

**Difficulty**: Easy  
**Tags**: `JOIN`, `GROUP BY`, `AGGREGATE`, `ROUND`  

---

## Table Schema

### `Project`

| Column Name  | Type | Description                                       |
|--------------|------|---------------------------------------------------|
| project_id   | int  | ID of the project                                 |
| employee_id  | int  | ID of an employee working on that project         |

- Primary Key: (`project_id`, `employee_id`)
- `employee_id` is a foreign key referencing the `Employee` table.

---

### `Employee`

| Column Name       | Type     | Description                            |
|-------------------|----------|----------------------------------------|
| employee_id       | int      | Unique ID of the employee              |
| name              | varchar  | Name of the employee                   |
| experience_years  | int      | Years of experience (guaranteed NOT NULL) |

---

## Objective

For each `project_id`, compute the **average number of years of experience** of employees working on that project.

- Round the average to 2 decimal places.
- If a project has no employees (though not expected), it should be ignored due to the inner join.
- Return in any order.

---

## SQL Query

```sql
SELECT 
    a.project_id,
    ROUND(AVG(b.experience_years), 2) AS average_years
FROM 
    Project a
INNER JOIN 
    Employee b 
    ON a.employee_id = b.employee_id
GROUP BY 
    a.project_id;
```
## Explanation 
```FROM Project a INNER JOIN Employee b ON a.employee_id = b.employee_id```
1. This line joins the Project table (aliased as a) with the Employee table (aliased as b) using employee_id as the key.
2. Only records that exist in both tables will be included (i.e., employees assigned to a project with matching employee details).
3. This is an INNER JOIN, so any employee listed in Project must have a corresponding entry in Employee.

```SELECT a.project_id, ...```
1. We're selecting the project_id column from the Project table.
2. This becomes the grouping key to calculate averages per project.

```AVG(b.experience_years)```
1. This function calculates the average years of experience of employees on each project.
2. Since experience_years is guaranteed to be NOT NULL, we don't need to worry about nulls affecting the result.
3. If it weren’t guaranteed, we could use AVG(CAST(experience_years AS FLOAT)) or wrap it in IFNULL().

```ROUND(..., 2) AS average_years```
1. SQL engines like MySQL return average values with many decimal places by default.
2. ROUND(..., 2) ensures that the average is displayed with exactly two digits after the decimal point, as required in the problem.

```GROUP BY a.project_id```
1. This clause groups the joined records by project_id, so that the average is computed per project, not across the entire table.
2. Without GROUP BY, the AVG() would return a single value for all projects combined, which is incorrect.
