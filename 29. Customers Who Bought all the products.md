# 29. Customers Who Bought All Products

## Problem Description

You are given two tables: `Customer` and `Product`.

### Customer Table

| Column Name  | Type |
|--------------|------|
| customer_id  | int  |
| product_key  | int  |

- This table may contain duplicate rows.
- `customer_id` is **not null**.
- `product_key` is a **foreign key** referencing the `Product` table.

### Product Table

| Column Name  | Type |
|--------------|------|
| product_key  | int  |

- `product_key` is the **primary key** in the Product table.

---

## Objective

Return the customer IDs of all customers who bought **all** the products listed in the `Product` table.

Return the result in any order.

---

## Example Input

```sql
Customer
+-------------+-------------+
| customer_id | product_key |
+-------------+-------------+
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
+-------------+-------------+

Product
+-------------+
| product_key |
+-------------+
| 5           |
| 6           |
+-------------+
```
## Output
```sql
+-------------+
| customer_id |
+-------------+
| 1           |
| 3           |
+-------------+
```

## SQL
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product);
```
## Explanation
1. ```GROUP BY customer_id```
→ Groups all purchases by each customer.

2. ```COUNT(DISTINCT product_key)```
→ Counts how many unique products the customer has purchased.

3. ```SELECT COUNT(*) FROM Product```
→ Gets the total number of products in the catalog.<br>
4. The HAVING clause ensures that only those customers who have bought all products (i.e., their count matches the total) are included in the result.
