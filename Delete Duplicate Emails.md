# 45. Delete Duplicate Emails

## Problem Statement

You are given a table named `Person` with the following schema:

| Column Name | Type    |
|-------------|---------|
| id          | int     |
| email       | varchar |

- `id` is the **primary key**, meaning it uniquely identifies each row.
- Each row in this table contains a lowercase email.
- You need to **delete all duplicate emails**, retaining only the **row with the smallest `id`** for each unique email.

### Example

**Input:**

```text
Person table:
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
```

**Output:**

```text
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```

**Explanation:**
- `john@example.com` appears twice (id 1 and 3).
- We keep the row with the smaller id (1) and delete the rest.

---

## SQL Solution

```sql
DELETE FROM Person 
WHERE id IN (
    SELECT a.id
    FROM (
        SELECT id, email,
               ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
        FROM Person
    ) a
    WHERE a.rn > 1
);
```

---

## Explanation

### Step-by-step breakdown:

1. **Subquery with `ROW_NUMBER()`**:
   ```sql
   SELECT id, email,
          ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
   FROM Person
   ```
   - This assigns a rank (`rn`) to each row **grouped by `email`**.
   - The email with the **smallest `id`** gets `rn = 1`.
   - Any duplicate emails will get `rn > 1`.

2. **Filter rows with duplicates**:
   ```sql
   WHERE a.rn > 1
   ```
   - These are the duplicate emails we want to delete.

3. **Use `id IN (...)` with DELETE**:
   - We wrap the subquery with an alias (`a`) because SQL does **not allow modifying and selecting from the same table in a subquery directly** — hence the inner derived table.

---





## 📌 Notes

- This method is preferable to using `GROUP BY` or `MIN(id)` when you want to **retain full row information** while removing duplicates.
- If your SQL engine supports it, you could use a **Common Table Expression (CTE)** instead of a derived table.
