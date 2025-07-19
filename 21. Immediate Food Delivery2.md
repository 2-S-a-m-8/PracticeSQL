# 21. Immediate Food Delivery II

**Difficulty:** Medium  
**Topic:** Aggregation, Filtering, Conditional Logic  
**Tags:** `GROUP BY`, `IF`, `ROUND`, `Subquery`

---

## Problem Description

You are given the `Delivery` table:

| Column Name                 | Type    |
|----------------------------|---------|
| delivery_id                | int     |
| customer_id                | int     |
| order_date                 | date    |
| customer_pref_delivery_date | date   |

- `delivery_id` is the unique ID for each delivery.
- Each order has a preferred delivery date.
- An **immediate** order is when `order_date == customer_pref_delivery_date`.
- A **scheduled** order is when preferred date is **after** order date.
- A customer’s **first order** is the one with the **earliest `order_date`**.
- Each customer has **exactly one first order**.

---

## Objective

Calculate the **percentage of immediate deliveries** **only among the first orders** of each customer.

- Round the result to **2 decimal places**.

---

## Example Input

### Delivery Table:

| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|------------------------------|
| 1           | 1           | 2019-08-01 | 2019-08-02                   |
| 2           | 2           | 2019-08-02 | 2019-08-02                   |
| 3           | 1           | 2019-08-11 | 2019-08-12                   |
| 4           | 3           | 2019-08-24 | 2019-08-24                   |
| 5           | 3           | 2019-08-21 | 2019-08-22                   |
| 6           | 2           | 2019-08-11 | 2019-08-13                   |
| 7           | 4           | 2019-08-09 | 2019-08-09                   |

---

## 📤 Expected Output

| immediate_percentage |
|----------------------|
| 50.00                |

---

## Approach

1. Identify the **first order** for each customer using `MIN(order_date)` and `GROUP BY customer_id`.
2. Filter the original `Delivery` table to retain only **those first orders**.
3. Use `IF(order_date = customer_pref_delivery_date, 1, 0)` to mark **immediate** orders.
4. Divide the sum of immediate orders by total first orders and **multiply by 100**.
5. Round the result to 2 decimal places.

---

## Final SQL Query

```sql
SELECT 
    ROUND(
        (SUM(IF(order_date = customer_pref_delivery_date, 1, 0)) / COUNT(*)) * 100, 
        2
    ) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
    SELECT customer_id, MIN(order_date)
    FROM Delivery
    GROUP BY customer_id
);
