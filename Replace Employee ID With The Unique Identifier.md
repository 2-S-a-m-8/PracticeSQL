### 06. Replace Employee ID With The Unique Identifier  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Joins  

---

### 🧩 Problem Statement  
**Table: Employees**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |

**id** is the primary key. Each row contains an employee's ID and name.

**Table: EmployeeUNI**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| unique_id   | int     |

**(id, unique_id)** is the primary key. Each row maps an employee's ID to a unique company-wide identifier.

---

### 🎯 Task  
Return a table with each employee's `unique_id` (if available) and `name`. If an employee does not have a `unique_id`, return `null` for it.

Return the result table in **any order**.

---

### 🧪 Example  

**Input:**  
Employees table:

| id  | name     |
|-----|----------|
| 1   | Alice    |
| 7   | Bob      |
| 11  | Meir     |
| 90  | Winston  |
| 3   | Jonathan |

EmployeeUNI table:

| id  | unique_id |
|-----|-----------|
| 3   | 1         |
| 11  | 2         |
| 90  | 3         |

**Output:**

| unique_id | name     |
|-----------|----------|
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |

---

### ✅ Solution  

```sql
SELECT b.unique_id, a.name
FROM Employees a
LEFT JOIN EmployeeUNI b
ON a.id = b.id;
```

---

### 🧠 Explanation  

- The `Employees` table has all employees' `id` and `name`.
- The `EmployeeUNI` table maps some `id`s to their `unique_id`s — but not all employees have a record here.
- To include **all employees**, whether they have a `unique_id` or not, we use a **LEFT JOIN**:
  - `LEFT JOIN` keeps **all rows** from the left table (`Employees`) and fills in data from the right table (`EmployeeUNI`) when there's a match.
  - If there's **no match**, the `unique_id` will appear as `NULL`.

So, for each employee:
- If they have a `unique_id` mapping, it appears.
- If not, the result shows `null`.

---
