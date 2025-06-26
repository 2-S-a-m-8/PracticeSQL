# 25. Product Sales Analysis III

**Difficulty:** Medium  
**Tags:** Aggregation, Grouping, Join

---

## Problem Description

You are given a table `Sales` that records sales transactions of products over different years.

### Table: `Sales`

| Column Name | Type |
|-------------|------|
| sale_id     | int  |
| product_id  | int  |
| year        | int  |
| quantity    | int  |
| price       | int  |

- `(sale_id, year)` is the primary key.
- `product_id` is a foreign key referring to the Product table.
- Each row represents a sale of a product in a given year.
- A product may have multiple sales entries in the same year.

---

## Objective

For each product, return **all sales entries** that occurred in the **first year** the product was sold.

Return a table with the following columns:  
`product_id`, `first_year`, `quantity`, `price`.

Return the result in **any order**.

---

## Example Input

**Sales Table:**

| sale_id | product_id | year | quantity | price |
|---------|------------|------|----------|-------|
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

---

## Expected Output

| product_id | first_year | quantity | price |
|------------|------------|----------|-------|
| 100        | 2008       | 10       | 5000  |
| 200        | 2011       | 15       | 9000  |

---

## SQL Query

```sql
SELECT 
    s.product_id,
    s.year AS first_year,
    s.quantity,
    s.price
FROM Sales s
JOIN (
    SELECT product_id, MIN(year) AS first_year
    FROM Sales
    GROUP BY product_id
) first_sales
ON s.product_id = first_sales.product_id 
   AND s.year = first_sales.first_year;
```
## Explanation
1. ```SELECT s.product_id, s.year AS first_year, s.quantity, s.price```
→ We retrieve the product details from the main Sales table.

2. ```JOIN (...) ON s.product_id = ... AND s.year = ...```
→ We find the minimum year each product appeared using a subquery.
→ Then we join that back to the Sales table to get all sales made during that first year.

3. ```MIN(year) in subquery```
→ Ensures we correctly identify the earliest year per product.

4. ```GROUP BY product_id```
→ Groups sales records so we can apply the MIN function per product.
---
