# 12. Students and Examinations

**Difficulty**: Easy  
**Tags**: `JOIN`, `GROUP BY`, `COUNT`, `CROSS JOIN`, `LEFT JOIN`  
**Topic**: SQL  

---

## 🧾 Problem Description

You are given three tables:

### `Students` Table
| Column Name   | Type    |
|---------------|---------|
| student_id    | int     |
| student_name  | varchar |

`student_id` is the primary key.

### `Subjects` Table
| Column Name   | Type    |
|---------------|---------|
| subject_name  | varchar |

`subject_name` is the primary key.

### `Examinations` Table
| Column Name   | Type    |
|---------------|---------|
| student_id    | int     |
| subject_name  | varchar |

There is **no primary key** for this table; it may contain duplicates.

Each student from the `Students` table takes every course from the `Subjects` table.  
Each row in `Examinations` indicates that a student attended an exam for that subject.

---

## Objective

Write a SQL query to **report the number of times** each student attended each exam.

### Output Schema:
| student_id | student_name | subject_name | attended_exams |
|------------|--------------|--------------|----------------|

Order the result by `student_id` and `subject_name`.

---

## Explanation of Approach

We need all combinations of students and subjects, **regardless of whether an exam was taken**.  
This means we should start with a **`CROSS JOIN`** of `Students` and `Subjects` to create the full matrix of possible `(student, subject)` pairs.

Then, we use a **`LEFT JOIN`** to attach the `Examinations` table (which has the actual attendance records), matching on both `student_id` and `subject_name`.

We then use `COUNT(c.student_id)` — which counts the number of matched records (i.e., number of exam attendances) — grouping by student and subject.

---

## SQL Query

```sql
SELECT 
    a.student_id,
    a.student_name,
    b.subject_name,
    COUNT(c.student_id) AS attended_exams
FROM Students a 
CROSS JOIN Subjects b
LEFT JOIN Examinations c 
    ON b.subject_name = c.subject_name 
   AND a.student_id = c.student_id
GROUP BY a.student_id, a.student_name, b.subject_name
ORDER BY a.student_id, b.subject_name;
