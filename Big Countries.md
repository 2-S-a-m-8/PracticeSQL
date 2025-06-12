### 03. Big Countries  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Filtering, Conditional Logic  

---

### 🧩 Problem Statement  
**Table: World**

| Column Name | Type    |
|-------------|---------|
| name        | varchar |
| continent   | varchar |
| area        | int     |
| population  | int     |
| gdp         | bigint  |

- `name` is the primary key.
- Each row gives information about a country's name, continent, area (in square kilometers), population, and GDP.

---

### ❓ Task  
A country is considered **big** if:

- It has an area of at least **3,000,000 km²**,  
**or**
- It has a population of at least **25,000,000**.

Write a SQL query to find the **name**, **population**, and **area** of such big countries.

Return the result in any order.

---

### 📊 Example  

**Input**

| name        | continent | area    | population | gdp          |
|-------------|-----------|---------|------------|--------------|
| Afghanistan | Asia      | 652230  | 25500100   | 20343000000  |
| Albania     | Europe    | 28748   | 2831741    | 12960000000  |
| Algeria     | Africa    | 2381741 | 37100000   | 188681000000 |
| Andorra     | Europe    | 468     | 78115      | 3712000000   |
| Angola      | Africa    | 1246700 | 20609294   | 100990000000 |

**Output**

| name        | population | area    |
|-------------|------------|---------|
| Afghanistan | 25500100   | 652230  |
| Algeria     | 37100000   | 2381741 |

---

### ✅ SQL Query with Explanation  

```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;
```

Let’s break down the logic:

1. SELECT name, population, area: <br>
We want to return these three columns for qualifying countries.
2. FROM World<br>
This tells SQL to pull data from the World table.
3. WHERE area >= 3000000 OR population >= 25000000<br>
We are filtering for countries with area ≥ 3 million square kilometers Or with a population ≥ 25 million people<br>
The OR operator ensures that either of the conditions is enough to include the country in the result.
