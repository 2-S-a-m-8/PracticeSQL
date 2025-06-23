# 19: Queries Quality and Percentage
**Difficulty**: Easy  
**Tags**:  `GROUP BY`, `AGGREGATE`, `ROUND` ,`CASE WHEN`, `AVG`
## Problem Statement

### Table: `Queries`

| Column Name | Type    |
|-------------|---------|
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |

- The `Queries` table contains logs of different queries with their results, position (from 1 to 500), and rating (from 1 to 5).
- **Duplicate rows may exist.**
- A **poor query** is one with `rating < 3`.

---

### Definitions

- **Query Quality**: Average of `(rating / position)` for a given `query_name`.
- **Poor Query Percentage**: `(number of poor queries / total queries) * 100`, rounded to two decimal places.

---

## Example Input

| query_name | result           | position | rating |
|------------|------------------|----------|--------|
| Dog        | Golden Retriever | 1        | 5      |
| Dog        | German Shepherd  | 2        | 5      |
| Dog        | Mule             | 200      | 1      |
| Cat        | Shirazi          | 5        | 2      |
| Cat        | Siamese          | 3        | 3      |
| Cat        | Sphynx           | 7        | 4      |

---

## Expected Output

| query_name | quality | poor_query_percentage |
|------------|---------|-----------------------|
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |

---

## 💡 SQL Solution

```sql
SELECT 
    query_name,
    ROUND(AVG(rating * 1.0 / position), 2) AS quality,
    ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS poor_query_percentage
FROM 
    Queries
GROUP BY 
    query_name;
```
## Explanation
```SELECT query_name, ...```
1. We are selecting query_name to group and display metrics per search query.
2. This will serve as the identifier for each query’s performance summary.
3. This column comes directly from the Queries table.

```ROUND(AVG(rating * 1.0 / position), 2) AS quality```
1. This expression computes the average of the ratio between the query's rating and its position in the result list.
2. rating * 1.0 ensures floating-point division (avoiding integer truncation).
3. AVG(...) calculates the mean ratio across all rows for the same query_name.
4. ROUND(..., 2) formats the result to two decimal places as required by the problem.

Why this works:
1. A higher rating and a lower position (i.e., ranked higher) lead to a higher quality.
2. For example, a rating of 5 at position 1 (5.0) is better than rating 3 at position 10 (0.3).

```ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS poor_query_percentage```
1. This part computes the percentage of poor queries, defined as having a rating below 3.
2. Breakdown:<br>
   a. CASE WHEN rating < 3 THEN 1 ELSE 0 END flags each poor query with a 1, and all others with 0.<br>
   b. SUM(...) counts how many poor queries exist for each query_name.<br>
   c. COUNT(*) gives the total number of rows per query.<br>
   d. Multiplying the ratio by 100.0 converts it into a percentage.<br>
   e. ROUND(..., 2) ensures the output is precise to two decimal points.<br>

3. Why use 100.0 instead of 100:This forces the engine to perform floating-point division rather than integer division, which avoids truncation and preserves decimal accuracy.
---

```FROM Queries```
Specifies the source table containing all records of queries and their metadata.
Each row represents an individual result associated with a query, including its rating and position.

```GROUP BY query_name```
This clause groups all rows by query_name so that the aggregate functions (AVG, SUM, COUNT) are calculated per query type.
Without this, the entire table would be treated as a single group, and we’d get one overall average and percentage instead of per-query values.
