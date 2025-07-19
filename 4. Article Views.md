# 04. Article Views I

## Table: Views

| Column Name | Type | Description |
|-------------|------|-------------|
| article_id  | int  | ID of the article |
| author_id   | int  | ID of the article's author |
| viewer_id   | int  | ID of the viewer |
| view_date   | date | Date of the view |

- There is no primary key.
- Each row shows that viewer_id viewed an article written by author_id on view_date.
- If author_id = viewer_id, the author viewed their own article.

---

## Task

Write a SQL query to find all authors who have viewed at least one of their own articles.

---

## Example

### Input:

Views

| article_id | author_id | viewer_id | view_date  |
|------------|-----------|-----------|------------|
| 1          | 3         | 5         | 2019-08-01 |
| 1          | 3         | 6         | 2019-08-02 |
| 2          | 7         | 7         | 2019-08-01 |
| 2          | 7         | 6         | 2019-08-02 |
| 4          | 7         | 1         | 2019-07-22 |
| 3          | 4         | 4         | 2019-07-21 |
| 3          | 4         | 4         | 2019-07-21 |

### Output:

| id  |
|-----|
| 4   |
| 7   |

---

## Explanation

We are looking for rows where author_id = viewer_id. These indicate that an author viewed their own article.

- Author 7 viewed their own article (row 3).
- Author 4 viewed their own article (rows 6 and 7).

We use DISTINCT to avoid duplicates and order the result by author ID.

---

## SQL Query

```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id;
