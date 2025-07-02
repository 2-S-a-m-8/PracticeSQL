# 36. Count Salary Categories

**Difficulty:** Medium  
**Tags:** Aggregation, Case Statement, UNION, Conditional Counting  

---

## Problem Description

Table: `Accounts`

| Column Name | Type |
|-------------|------|
| account_id  | int  |
| income      | int  |

- `account_id` is the **primary key**.
- Each row contains information about the **monthly income** for one bank account.

---

## Goal

Return the number of bank accounts in each of the following **salary categories**:

- **"Low Salary"**: income **< 20000**
- **"Average Salary"**: **20000 ≤ income ≤ 50000**
- **"High Salary"**: income **> 50000**

The result must include all three categories. If there are no accounts in a category, return 0.

---

## Example

### Input

**Accounts Table:**

| account_id | income |
|------------|--------|
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |

### Output

| category       | accounts_count |
|----------------|----------------|
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |

### Explanation

- Low Salary: Account 2 has income < 20000  
- Average Salary: No accounts  
- High Salary: Accounts 3, 6, and 8

---

## SQL Query 

```sql
(
  SELECT 
    'Low Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income < 20000) AS accounts_count
)
UNION ALL
(
  SELECT 
    'Average Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income BETWEEN 20000 AND 50000) AS accounts_count
)
UNION ALL
(
  SELECT 
    'High Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income > 50000) AS accounts_count
);
```
---
## Explanation
```sql
(
  SELECT 
    'Low Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income < 20000) AS accounts_count
)
```
- Creates a single row with the category label 'Low Salary'.
- The subquery: ```SELECT COUNT(*) FROM Accounts WHERE income < 20000``` counts how many rows (i.e., accounts) have income below 20000.
```sql
  
| category    | accounts_count |
|-------------|----------------|
| Low Salary  | (count value)  |

```
```sql
UNION ALL
```
- Combines the rows from each of the 3 SELECT statements.
- UNION ALL is used instead of UNION to ensure all three rows are preserved and not de-duplicated.
```sql (
  SELECT 
    'Average Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income BETWEEN 20000 AND 50000) AS accounts_count
)
```
- Again, creates a single row labeled 'Average Salary'.
- Subquery counts accounts with income between 20000 and 50000 (inclusive).
```sql
UNION ALL
(
  SELECT 
    'High Salary' AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income > 50000) AS accounts_count
);
```
- Final category: 'High Salary'
- Subquery counts how many accounts earn more than 50000.
---
