### 07. Product Sales Analysis I  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Joins  

---

### 🧩 Problem Statement  
**Table: Sales**

| Column Name | Type  |
|-------------|-------|
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |

(sale_id, year) is the primary key. product_id is a foreign key to the Product table.

**Table: Product**

| Column Name  | Type    |
|--------------|---------|
| product_id   | int     |
| product_name | varchar |

product_id is the primary key. Each row of this table indicates the product name of each product.

---

### 🎯 Task  
Return a table with each product's `product_name`, `year`, and `price` based on the information in the `Sales` table.  

Return the result table in **any order**.

---

### 🧪 Example  

**Input:**  
Sales table:

| sale_id | product_id | year | quantity | price |
|---------|------------|------|----------|-------|
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |

Product table:

| product_id | product_name |
|------------|--------------|
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |

**Output:**  

| product_name | year | price |
|--------------|------|-------|
| Nokia        | 2008 | 5000  |
| Nokia        | 2009 | 5000  |
| Apple        | 2011 | 9000  |

---

### ✅ Solution  

```sql
SELECT 
    b.product_name, 
    a.year, 
    a.price
FROM 
    Sales a
INNER JOIN 
    Product b 
ON 
    a.product_id = b.product_id;

```
### Explanation
1. The Sales table contains the sales records with product_id, year, and price, but it doesn't store the product names.
2. The Product table maps product_id to product_name.
3. To get the product_name for each sale, we perform an INNER JOIN on the shared column product_id.
4. This join includes only those sales that have a matching entry in the Product table.
5. We then select the desired columns: product_name, year, and price.
