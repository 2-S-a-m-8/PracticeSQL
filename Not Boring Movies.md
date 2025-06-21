# 15. Not Boring Movies

**Difficulty**: Easy
**Tags**: `STRING`, `WHERE` 
**Topic**: SQL  

## Problem Statement

You are given a table `Cinema` with the following schema:

| Column Name  | Type     |
|--------------|----------|
| id           | int      |
| movie        | varchar  |
| description  | varchar  |
| rating       | float    |

- `id` is the primary key.
- Each row contains the name of a movie, its genre/description, and its rating.
- Ratings are floating-point numbers rounded to 2 decimal places and range from 0 to 10.

### Task

Write a SQL query to report movies that:

- Have an **odd-numbered id**
- Whose `description` is **not exactly `'boring'`**
- Return the result ordered by `rating` **in descending order**

## Input Example

**Cinema Table:**

| id | movie      | description | rating |
|----|------------|-------------|--------|
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | Irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |

## Output Example

| id | movie      | description | rating |
|----|------------|-------------|--------|
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |


---

### ✅ SQL Query:

```sql
SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 != 0
  AND description NOT LIKE 'boring'
ORDER BY rating DESC;
```

### Step-by-Step Breakdown:

1. SELECT Clause

```SELECT id, movie, description, rating```
Retrieves the columns we want in the result.

2. FROM Clause

```FROM Cinema```
Specifies the source table.

3. WHERE Clause

```WHERE id % 2 != 0 AND description NOT LIKE 'boring'```
id % 2 != 0: Filters to odd-numbered ids.
description NOT LIKE 'boring': Excludes rows with exact description 'boring' (case-sensitive and no wildcards used).

4. ORDER BY Clause

```ORDER BY rating DESC;```
Sorts the output by rating in descending order (highest-rated movies first).
