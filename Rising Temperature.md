### 09. Rising Temperature  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Self Join, Date Arithmetic  
**Tags:** `Self Join`, `Time Series`, `Date Logic`

---

### Problem Statement  
**Table: Weather**

| Column Name   | Type    |
|---------------|---------|
| id            | int     |
| recordDate    | date    |
| temperature   | int     |

- `id` is the primary key.
- Each row contains the temperature on a given day.
- There are no duplicate dates.

---

### Goal  
Find all `id`s where the temperature was **higher than the previous day's temperature**.  
Ignore rows where there is **no previous day** (i.e. date is missing).

---

### Example Input  

**Weather Table:**

| id | recordDate | temperature |
|----|------------|-------------|
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |

---

### Expected Output  

| id |
|----|
| 2  |
| 4  |

**Explanation:**
- Day 2 (25°C) > Day 1 (10°C) → include id `2`
- Day 4 (30°C) > Day 3 (20°C) → include id `4`

---

### Edge Case  

**Weather Table:**

| id | recordDate | temperature |
|----|------------|-------------|
| 1  | 2000-12-14 | 3           |
| 2  | 2000-12-16 | 5           |

→ No row for 2000-12-15, so day 16 is not compared. Output should be **empty**. If dates were continuous we could have used LAG efficiently

---

### ✅ Correct SQL Solution  

```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2 
  ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;
```
### Explanation
1. w1 represents today, w2 represents yesterday.
2. DATEDIFF(w1.recordDate, w2.recordDate) = 1 ensures an exact 1-day gap.
3. Compare w1.temperature > w2.temperature.
4. This avoids incorrect comparisons when days are skipped in the dataset.
