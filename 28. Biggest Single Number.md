# 28. Biggest Single Number

## Problem Description

You are given a table `MyNumbers` with a single column:

| Column Name | Type |
|-------------|------|
| num         | int  |

Each row contains an integer. The same number may appear multiple times.

A **single number** is defined as a number that appears exactly **once** in the table.

Your task is to **find the largest single number**. If no single number exists, return `null`.

---

## Example Input 1

```sql
MyNumbers
+-----+
| num |
+-----+
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |
+-----+
```
## Output
```sql
+-----+
| num |
+-----+
| 6   |
+-----+
```

## Example Input 2
MyNumbers
```sql
+-----+
| num |
+-----+
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |
+-----+
```
## Output
```sql
+------+
| num  |
+------+
| null |
+------+
```
## SQL Solution
```sql WITH temp AS (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
)
SELECT MAX(num) AS num
FROM temp;
```

## Explanation
1. GROUP BY num: groups each number to count how many times it appears.
2. HAVING COUNT(num) = 1: filters only those that appear exactly once.
3. MAX(num): returns the largest of these "single" numbers.
4. If no such numbers exist, it returns NULL.
