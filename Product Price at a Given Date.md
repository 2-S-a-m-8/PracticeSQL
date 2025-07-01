# 34. Product Price at a Given Date

## Problem Description

**Table:** `Products`

| Column Name   | Type    |
|---------------|---------|
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |

- `(product_id, change_date)` is the **primary key**.
- Each row records a price change of a product on a given date.
- Initially, **all products had a price of 10**.

Write a query to find the prices of **all products on the date `'2019-08-16'`**.

Return the result table in **any order**.

---

## Example

**Input:**

`Products` table:

| product_id | new_price | change_date |
|------------|-----------|-------------|
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |

**Output:**

| product_id | price |
|------------|-------|
| 1          | 35    |
| 2          | 50    |
| 3          | 10    |

---

## Explanation

- Product `1`: last change **on or before** `'2019-08-16'` → price is **35**
- Product `2`: last change before `'2019-08-17'` is on `'2019-08-14'` → price is **50**
- Product `3`: no price change **before or on** `'2019-08-16'` → default price is **10**

---

## SQL Query

```sql
SELECT DISTINCT product_id, 10 AS price
FROM Products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id
    FROM Products
    WHERE change_date <= '2019-08-16'
)

UNION

SELECT product_id, new_price AS price
FROM Products
WHERE (product_id, change_date) IN (
    SELECT product_id, MAX(change_date) AS date
    FROM Products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
);
```
---
## Explanation
1. Handle products with no price changes before or on 2019-08-16<br>
```sql
SELECT DISTINCT product_id, 10 AS price
FROM Products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id
    FROM Products
    WHERE change_date <= '2019-08-16'
)
```
- This subquery finds products not present in the list of products that had a price change on or before '2019-08-16'.
- For these products, we assume their price is still the initial value of 10.
- We use DISTINCT to avoid duplicates.
  
2. Handle products with at least one price change on or before the date<br>
```sql
SELECT product_id, new_price AS price
FROM Products
WHERE (product_id, change_date) IN (
    SELECT product_id, MAX(change_date) AS date
    FROM Products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)
```
- The inner subquery finds the latest price change date per product up to '2019-08-16'.<br>
```sql
SELECT product_id, MAX(change_date)
FROM Products
WHERE change_date <= '2019-08-16'
GROUP BY product_id
```
- The outer query then selects the new_price for each (product_id, change_date) combination from that latest date.
- This gives us the most recent price for each product up to and including '2019-08-16'.
3. Combine both parts using Union
---
