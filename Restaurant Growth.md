# 40. Restaurant Growth

**Difficulty:** Medium  
**Topic:** Window Functions, Aggregation  
**Tags:** SQL, Analytics, Rolling Average

---

## Table Schema

**Customer**

| Column Name   | Type    | Description                              |
|---------------|---------|------------------------------------------|
| customer_id   | int     | Unique ID of the customer                |
| name          | varchar | Name of the customer                     |
| visited_on    | date    | Date of visit                            |
| amount        | int     | Amount spent by the customer             |

Primary Key: `(customer_id, visited_on)`

---

## Problem Statement

You are the restaurant owner and want to analyze customer spending to consider expansion.

Calculate the **7-day moving average** of the total `amount` paid, including the current day and the 6 previous days. Round the average to **two decimal places**.

Return the results sorted by `visited_on` in ascending order. Only include rows from the **7th day onward** (i.e., complete 7-day window available).

---

## Example Input

**Customer Table:**

| customer_id | name    | visited_on | amount |
|-------------|---------|------------|--------|
| 1           | Jhon    | 2019-01-01 | 100    |
| 2           | Daniel  | 2019-01-02 | 110    |
| 3           | Jade    | 2019-01-03 | 120    |
| 4           | Khaled  | 2019-01-04 | 130    |
| 5           | Winston | 2019-01-05 | 110    |
| 6           | Elvis   | 2019-01-06 | 140    |
| 7           | Anna    | 2019-01-07 | 150    |
| 8           | Maria   | 2019-01-08 | 80     |
| 9           | Jaze    | 2019-01-09 | 110    |
| 1           | Jhon    | 2019-01-10 | 130    |
| 3           | Jade    | 2019-01-10 | 150    |

---

## Expected Output

| visited_on | amount | average_amount |
|------------|--------|----------------|
| 2019-01-07 | 860    | 122.86         |
| 2019-01-08 | 840    | 120.00         |
| 2019-01-09 | 840    | 120.00         |
| 2019-01-10 | 1000   | 142.86         |

---

## Approach

1. Use `SUM(amount)` with a **window function** to get the 7-day total.
2. Define the window using `RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW`.
3. Round the result to two decimal places for `average_amount`.
4. Use `DISTINCT visited_on` to avoid duplicate rows per date.
5. Use `LIMIT 6, 999` to skip the first 6 days (insufficient window).

---

## SQL Query

```sql
SELECT DISTINCT visited_on,
       SUM(amount) OVER w AS amount,
       ROUND(SUM(amount) OVER w / 7, 2) AS average_amount
FROM Customer
WINDOW w AS (
    ORDER BY visited_on
    RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW
)
LIMIT 6, 999;
```
---
