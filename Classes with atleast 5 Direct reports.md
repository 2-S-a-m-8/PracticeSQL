# 26. Classes With at Least 5 Students

**Difficulty:** Easy  
**Tags:** Aggregation, Grouping

---

## Problem Description

You are given a table `Courses`, where each row represents a student enrolled in a class.

### Table: `Courses`

| Column Name | Type    |
|-------------|---------|
| student     | varchar |
| class       | varchar |

- The combination `(student, class)` is the **primary key**.
- Each row represents that a specific student is taking a specific class.

---

## Objective

Return all classes that have **at least 5 students**.

You may return the result in **any order**.

---

## Example Input

**Courses Table:**

| student | class    |
|---------|----------|
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |

---

## Expected Output

| class   |
|---------|
| Math    |

---

## SQL Query

```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```
## Explanation
1. GROUP BY class: Groups all rows by the class name.
2. COUNT(DISTINCT student): Counts the number of unique students in each class.
3. HAVING ... >= 5: Filters only those groups (classes) where the count is 5 or more.<br>
***This ensures only classes with 5 or more unique students are included.***
