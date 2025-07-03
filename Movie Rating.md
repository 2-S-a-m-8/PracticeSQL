# 39. Movie Rating

## Difficulty: Medium  
**Topic:** Aggregate Functions, GROUP BY, JOIN, Date Filtering  
**Company Tags:** Amazon, Microsoft, Google  

---

## Table Definitions

### Table: `Movies`

| Column Name | Type    |
|-------------|---------|
| movie_id    | int     |
| title       | varchar |

- `movie_id` is the primary key.
- Each row indicates a movie title.

---

### Table: `Users`

| Column Name | Type    |
|-------------|---------|
| user_id     | int     |
| name        | varchar |

- `user_id` is the primary key.
- `name` is unique for each user.

---

### Table: `MovieRating`

| Column Name | Type    |
|-------------|---------|
| movie_id    | int     |
| user_id     | int     |
| rating      | int     |
| created_at  | date    |

- `(movie_id, user_id)` is the primary key.
- This table contains a user's rating for a movie and when it was submitted.

---

## Problem Statement

Write a SQL query to:
1. Find the **name of the user** who has rated the **greatest number of movies**.  
   - In case of a tie, return the lexicographically smaller name.

2. Find the **title of the movie** with the **highest average rating in February 2020**.  
   - In case of a tie, return the lexicographically smaller title.

Return a single column named `results` with the two output values as two rows.

---

## Input Example

```text
Movies:
+-----------+------------+
| movie_id  | title      |
+-----------+------------+
| 1         | Avengers   |
| 2         | Frozen 2   |
| 3         | Joker      |
+-----------+------------+

Users:
+----------+---------+
| user_id  | name    |
+----------+---------+
| 1        | Daniel  |
| 2        | Monica  |
| 3        | Maria   |
| 4        | James   |
+----------+---------+

MovieRating:
+-----------+----------+--------+------------+
| movie_id  | user_id  | rating | created_at |
+-----------+----------+--------+------------+
| 1         | 1        | 3      | 2020-01-12 |
| 1         | 2        | 4      | 2020-02-11 |
| 1         | 3        | 2      | 2020-02-12 |
| 1         | 4        | 1      | 2020-01-01 |
| 2         | 1        | 5      | 2020-02-17 |
| 2         | 2        | 2      | 2020-02-01 |
| 2         | 3        | 2      | 2020-03-01 |
| 3         | 1        | 3      | 2020-02-22 |
| 3         | 2        | 4      | 2020-02-25 |
+-----------+----------+--------+------------+
```
## Output Example

```text
+----------+
| results  |
+----------+
| Daniel   |
| Frozen 2 |
+----------+
```
## SQL Query

```sql
(
  SELECT name AS results
  FROM MovieRating
  JOIN Users USING(user_id)
  GROUP BY name
  ORDER BY COUNT(*) DESC, name
  LIMIT 1
)
UNION ALL
(
  SELECT title AS results
  FROM MovieRating
  JOIN Movies USING(movie_id)
  WHERE EXTRACT(YEAR_MONTH FROM created_at) = 202002
  GROUP BY title
  ORDER BY AVG(rating) DESC, title
  LIMIT 1
);
```

## Explanation
1. PART 1
```sql
(
  SELECT name AS results
  FROM MovieRating
  JOIN Users USING(user_id)
  GROUP BY name
  ORDER BY COUNT(*) DESC, name
  LIMIT 1
)
```
- ```FROM MovieRating JOIN Users USING(user_id)``` -> We join MovieRating and Users on the user_id column. This lets us map user_id to name (we need name in output).
- ```GROUP BY name``` -> We group the data by user name. Now each row represents one user and the ratings they've made.
- ```ORDER BY COUNT(*) DESC, name``` -> COUNT(*) counts how many rows (i.e., ratings) each user made. We sort in descending order to get the user with the most ratings on top. If there's a tie (e.g., Daniel and Monica rated 3 movies each), we use name in ascending order to break it (lexicographically).
- ```LIMIT 1``` -> Picks only one user (the top one after sorting).
- ```SELECT name AS results``` -> The selected name is labeled as results for final output.<br>
2. PART 2
```sql
(
  SELECT title AS results
  FROM MovieRating
  JOIN Movies USING(movie_id)
  WHERE EXTRACT(YEAR_MONTH FROM created_at) = 202002
  GROUP BY title
  ORDER BY AVG(rating) DESC, title
  LIMIT 1
)
```
- ```FROM MovieRating JOIN Movies USING(movie_id)``` -> We join MovieRating and Movies tables on movie_id to get the movie titles alongside ratings.
- ```WHERE EXTRACT(YEAR_MONTH FROM created_at) = 202002``` -> This filters only ratings in February 2020. EXTRACT(YEAR_MONTH FROM created_at) converts 2020-02-17 to 202002.
- ```GROUP BY title``` -> Groups the ratings by movie title so we can compute average ratings.
- ```ORDER BY AVG(rating) DESC, title``` -> Sort by average rating (highest first). In case of tie (e.g., Frozen 2 and Joker both have average 3.5), we sort by title lexicographically.
- ``` LIMIT 1``` -> Pick the top movie after sorting.
- ```SELECT title AS results``` -> Return just the movie title as results.
- ```UNION ALL``` -> Combines the two SELECT statements into a single result. UNION ALL keeps both rows, no deduplication.
---
