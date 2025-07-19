# 01. Recyclable and Low Fat Products

**Difficulty**: Easy  
**Platform**: LeetCode  
**Topics**: SQL, Filtering, Boolean Logic

---

## Problem Statement

**Table**: `Products`

| Column Name | Type         |
|-------------|--------------|
| product_id  | int          |
| low_fats    | enum('Y','N')|
| recyclable  | enum('Y','N')|

- `product_id` is the primary key.
- `low_fats` indicates whether a product is low fat (`'Y'` = Yes, `'N'` = No).
- `recyclable` indicates whether a product is recyclable (`'Y'` = Yes, `'N'` = No).

---

## ❓ Task

Write a SQL query to return the product IDs of all products that are **both** low fat and recyclable.

Return the result in any order.

---

## Example

### Input

| product_id | low_fats | recyclable |
|------------|----------|------------|
| 0          | Y        | N          |
| 1          | Y        | Y          |
| 2          | N        | Y          |
| 3          | Y        | Y          |
| 4          | N        | N          |

### Output

| product_id |
|------------|
| 1          |
| 3          |

---

## SQL Query with Explanation

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```
---

### Explanation

Let’s break down the query step by step:

1. SELECT product_id<br>
This tells SQL that we want to return only the product_id column from the result.
We're not interested in the other columns like low_fats or recyclable in the output — just the IDs of qualifying products.

2. FROM Products<br>
This specifies the table we're pulling the data from.
In this case, the table is named Products, which contains information about different products and their attributes.

3. WHERE low_fats = 'Y' AND recyclable = 'Y'<br>
This is the filter condition that selects only the rows where both conditions are true:<br>
The product must be low fat, which is checked by low_fats = 'Y'.<br>
The product must also be recyclable, checked by recyclable = 'Y'.<br>

The keyword AND ensures that both conditions must be satisfied for a product to be included in the result.
If even one condition fails, the product is excluded.
