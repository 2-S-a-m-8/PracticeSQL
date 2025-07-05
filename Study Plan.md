# 41. Investments in 2016

## Problem Statement

Given a table `Insurance`, report the **sum of all total investment values in 2016** (`tiv_2016`), for all policyholders who:

1. Have the **same `tiv_2015`** as one or more other policyholders.
2. Are located in a **unique city** (i.e., their `(lat, lon)` pair is **not shared** by any other policyholder).

The result should be rounded to **two decimal places**.

---

## Schema

```sql
Table: Insurance

+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| pid         | int    | -- Primary Key
| tiv_2015    | float  |
| tiv_2016    | float  |
| lat         | float  |
| lon         | float  |
+-------------+--------+
```

---

## Example Input

```sql
Insurance Table:
+-----+----------+----------+-----+-----+
| pid | tiv_2015 | tiv_2016 | lat | lon |
+-----+----------+----------+-----+-----+
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |
+-----+----------+----------+-----+-----+
```

---

## Output

```sql
+----------+
| tiv_2016 |
+----------+
| 45.00    |
+----------+
```

---



## ✅ Final Query

```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND CONCAT(lat, lon) NOT IN (
    SELECT CONCAT(lat, lon)
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) > 1
);
```

---

## Explanation
### Main SELECT Clause:

```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
```

- **What it does:** Sums up the valid `tiv_2016` values and **rounds** the result to 2 decimal places.
- **Why ROUND?** Because the problem requires the result formatted to 2 decimal places.

---

### First Filter Condition: tiv_2015 is not unique

```sql
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
```

- **Purpose:** Only include rows where the `tiv_2015` value is **shared by at least one other person**.
- **Explanation:**
  - `GROUP BY tiv_2015` groups all records by `tiv_2015`.
  - `HAVING COUNT(*) > 1` filters only those `tiv_2015` values that appear more than once.
  - So this subquery returns a list of `tiv_2015` values that are **duplicates**.

---

### Second Filter Condition: Unique Location Check

```sql
AND CONCAT(lat, lon) NOT IN (
    SELECT CONCAT(lat, lon)
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) > 1
)
```

- **Purpose:** Exclude any row whose `(lat, lon)` coordinates **appear more than once**.
- **Explanation:**
  - `CONCAT(lat, lon)` merges latitude and longitude to create a unique identifier for each location.
  - `GROUP BY lat, lon` groups by exact coordinates.
  - `HAVING COUNT(*) > 1` filters locations that **appear multiple times**, i.e., **shared cities**.
  - `NOT IN (...)` ensures that only **unique city locations** are selected.

> Note: Using `CONCAT(lat, lon)` works because both lat and lon are guaranteed to be non-null. Alternatively, you can use tuples like `(lat, lon)` in some SQL dialects.


---
