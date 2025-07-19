# 20. Monthly Transactions I

**Difficulty:** Medium  
**Topic:** Aggregation, Grouping, Filtering  
**Tags:** `GROUP BY`, `CASE`, `SUM`, `COUNT`, `DATE_FORMAT`

---

## Problem Description

You are given a `Transactions` table with the following schema:

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| country     | varchar |
| state       | enum    |
| amount      | int     |
| trans_date  | date    |

- Each row is an incoming transaction.
- `state` is either `'approved'` or `'declined'`.

---

## Objective

For **each month and country**, return:

- Total number of transactions
- Number of approved transactions
- Total transaction amount
- Approved transaction amount

---

## Example Input

### Transactions Table:

| id  | country | state    | amount | trans_date |
|-----|---------|----------|--------|------------|
| 121 | US      | approved | 1000   | 2018-12-18 |
| 122 | US      | declined | 2000   | 2018-12-19 |
| 123 | US      | approved | 2000   | 2019-01-01 |
| 124 | DE      | approved | 2000   | 2019-01-07 |

---

## Expected Output

| month   | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
|---------|---------|--------------|----------------|---------------------|------------------------|
| 2018-12 | US      | 2            | 1              | 3000                | 1000                   |
| 2019-01 | US      | 1            | 1              | 2000                | 2000                   |
| 2019-01 | DE      | 1            | 1              | 2000                | 2000                   |

---

## Approach

1. **Format the date** to extract year and month using `DATE_FORMAT(trans_date, '%Y-%m')`.
2. **Group** by this formatted month and `country`.
3. **Count** all transactions using `COUNT(state)`.
4. Use `CASE WHEN` inside `SUM` to:
   - Count only `approved` transactions.
   - Sum only `approved` amounts.

---

## Final SQL Query

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month, 
    country,
    COUNT(state) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
