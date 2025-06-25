# 23. Number of Unique Subjects Taught by Each Teacher

**Difficulty:** Easy  
**Tags:** Aggregation, Grouping

---

## Problem Description

You are given a table `Teacher`, where each row indicates that a teacher teaches a specific subject in a specific department.

### Table: `Teacher`

| Column Name | Type |
|-------------|------|
| `teacher_id` | int  |
| `subject_id` | int  |
| `dept_id`    | int  |

- The combination `(subject_id, dept_id)` is a **primary key**, meaning each subject-department pair is unique.
- A teacher may teach the same subject in multiple departments.

---

## Objective

For each teacher, report the **number of unique subjects** they teach, **regardless of department**.

Return the result in any order.

---

## Example Input

**Teacher Table:**

| teacher_id | subject_id | dept_id |
|------------|------------|---------|
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |

---

## Expected Output

| teacher_id | cnt |
|------------|-----|
| 1          | 2   |
| 2          | 4   |

---

## SQL Query

```sql
SELECT 
    teacher_id, 
    COUNT(DISTINCT subject_id) AS cnt
FROM 
    Teacher
GROUP BY 
    teacher_id;
```
## Explanation
1. ```SELECT teacher_id```
→ We want to display results for each teacher, so we include the teacher_id in the SELECT clause.

2. ```COUNT(DISTINCT subject_id)```
→ For each teacher, we count how many different subjects they teach.
→ We use DISTINCT because the same subject might appear in multiple departments, and we only want to count it once.

3. ```FROM Teacher```
→ We're querying from the Teacher table provided in the problem.

4. ```GROUP BY teacher_id```
→ This groups the rows by teacher_id, so that the COUNT(DISTINCT subject_id) is calculated separately for each teacher.

---
