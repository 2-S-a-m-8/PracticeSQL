### 02. Find Customer Referee  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Filtering, `NULL` Handling, Boolean Logic  

---

### 🧩 Problem Statement  
**Table: Customer**

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| name        | varchar |
| referee_id  | int     |

- `id` is the primary key for this table.
- Each row indicates the customer’s ID, their name, and the ID of the customer who referred them.
- A `referee_id` can be `NULL` if the customer was not referred by anyone.

---

### ❓ Task  
Write a SQL query to find the **names of customers who are *not* referred by the customer with `id = 2`**.

Return the result table in any order.

---

### 📊 Example  

**Input**

| id | name | referee_id |
|----|------|------------|
| 1  | Will | null       |
| 2  | Jane | null       |
| 3  | Alex | 2          |
| 4  | Bill | null       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

**Output**

| name |
|------|
| Will |
| Jane |
| Bill |
| Zack |

---

### ✅ SQL Query with Explanation  
```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```
---

### Explanation

Let’s break down the logic:

1. SELECT name:<br>
We only want to retrieve the name of the customers.<br>

2. FROM Customer:<br>
The data source is the Customer table.<br>

3. WHERE referee_id != 2 OR referee_id IS NULL:<br>
This condition ensures we exclude any customer who was referred by the customer with id = 2.
We use referee_id IS NULL to include those who were not referred by anyone.
!= 2 includes all customers referred by someone other than customer 2.
---
