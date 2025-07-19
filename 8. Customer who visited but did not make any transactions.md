### 08. Customer Who Visited but Did Not Make Any Transactions  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Joins, Aggregation  

---

### 🧩 Problem Statement  

**Table: Visits**

| Column Name | Type  |
|-------------|-------|
| visit_id    | int   |
| customer_id | int   |

visit_id is the primary key of this table.  
Each row indicates that a customer with `customer_id` visited the mall.

**Table: Transactions**

| Column Name    | Type  |
|----------------|-------|
| transaction_id | int   |
| visit_id       | int   |
| amount         | int   |

transaction_id is the primary key of this table.  
Each row indicates a transaction made during a particular visit.

---

### Task  
Write a query to find the customer IDs of users who visited the mall but did not make any transactions, and the number of such visits per customer.  

Return the result with the following columns:  
- `customer_id`  
- `count_no_trans`  

Return the result in any order.

---

### Example  

**Input:**  

Visits table:

| visit_id | customer_id |
|----------|-------------|
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |

Transactions table:

| transaction_id | visit_id | amount |
|----------------|----------|--------|
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |

**Output:**  

| customer_id | count_no_trans |
|-------------|----------------|
| 30          | 1              |
| 96          | 1              |
| 54          | 2              |

---

### Solution  

```sql
SELECT 
    v.customer_id,
    COUNT(*) AS count_no_trans
FROM 
    Visits v
LEFT JOIN 
    Transactions t
ON 
    v.visit_id = t.visit_id
WHERE 
    t.visit_id IS NULL
GROUP BY 
    v.customer_id;
```
### Explanation

1. We use a LEFT JOIN to include all rows from Visits even if there are no matching entries in Transactions.
2. The condition t.visit_id IS NULL filters out visits that had transactions. The result now only includes visits with no transactions.
3. We group by customer_id to count how many such visits each customer made.
4. The final output includes customer_id and the corresponding count of visits with no transactions.
