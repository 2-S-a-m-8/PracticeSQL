# 32. Triangle Judgement

## Problem Statement

You are given a table named `Triangle` with the following schema:

| Column Name | Type |
|-------------|------|
| x           | int  |
| y           | int  |
| z           | int  |

Each row contains the lengths of three line segments. You need to determine whether these segments can form a **valid triangle**.
---

### Triangle Validity Condition

A triangle is **valid** if the **sum of the lengths of any two sides is greater than the third**:

- x + y > z  
- x + z > y  
- y + z > x  

This is equivalent to checking:

```sql
x < y + z AND y < x + z AND z < x + y
```
---
## Objective
Return a result table containing columns x, y, z, and an extra column triangle with values:
1. 'Yes' if the segments form a valid triangle
2. 'No' otherwise
<br>You can return the result in any order.
---
## Example
### Input
```sql
Triangle table:
+----+----+----+
| x  | y  | z  |
+----+----+----+
| 13 | 15 | 30 |
| 10 | 20 | 15 |
+----+----+----+
```
---
### Output
```sql
+----+----+----+----------+
| x  | y  | z  | triangle |
+----+----+----+----------+
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |
+----+----+----+----------+
```
---
## Query
```sql
SELECT 
    x, y, z,
    CASE 
        WHEN x < y + z AND y < x + z AND z < x + y THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM 
    Triangle;
```
---
# Explanation
1. CASE statement is used to label each row as 'Yes' or 'No'.
2. The condition checks if all three triangle inequalities are satisfied.
3. If true: the row is marked 'Yes', else 'No'.
---
