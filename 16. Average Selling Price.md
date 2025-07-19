# 16. Average Selling Price

**Difficulty**: Easy  
**Tags**: `JOIN`, `GROUP BY`, `AGGREGATE`, `ROUND`, `IFNULL`  

---

## Table Schema

### `Prices`

| Column Name | Type    | Description                                          |
|-------------|---------|------------------------------------------------------|
| product_id  | int     | ID of the product                                    |
| start_date  | date    | Start of the price period                            |
| end_date    | date    | End of the price period                              |
| price       | int     | Price of the product during that time period         |

- Primary Key: (`product_id`, `start_date`, `end_date`)
- No overlapping periods for the same product

### `UnitsSold`

| Column Name   | Type    | Description                          |
|---------------|---------|--------------------------------------|
| product_id    | int     | ID of the product sold               |
| purchase_date | date    | Date of the sale                     |
| units         | int     | Number of units sold on that date    |

---

## Objective

For each `product_id`, calculate the **average selling price** using: average_price = (sum of price * units sold) / total units sold

- Round the result to **2 decimal places**
- If a product has **no sales**, its average price should be `0`

---

## SQL Solution

```sql
SELECT 
    a.product_id,
    IFNULL(ROUND(SUM(a.price * b.units) / SUM(b.units), 2), 0) AS average_price
FROM 
    Prices a
LEFT JOIN 
    UnitsSold b 
    ON a.product_id = b.product_id
    AND b.purchase_date BETWEEN a.start_date AND a.end_date
GROUP BY 
    a.product_id;
```
## Explanation

1. We LEFT JOIN Prices with UnitsSold using: Matching product_id, purchase_date falls between start_date and end_date (price was active)
2. SUM(a.price * b.units) gives total revenue
3. SUM(b.units) gives total units sold
4. The division gives the average price
5. ROUND(..., 2) to round to 2 decimal places
6. IFNULL(..., 0) ensures products with no sales get 0 instead of NULL
---



