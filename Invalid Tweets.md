### 05. Invalid Tweets  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Topics:** SQL, Filtering, String Functions  

---

### 🧩 Problem Statement  
**Table: Tweets**

| Column Name | Type    |
|-------------|---------|
| tweet_id    | int     |
| content     | varchar |

- `tweet_id` is the primary key.
- Each row in this table contains a tweet's ID and its text content.
- A tweet is considered **invalid** if the number of characters in its content is **strictly greater than 15**.

---

### ❓ Task  
Write a SQL query to find the **IDs** of all tweets that are **invalid** — i.e., tweets with **content length > 15 characters**.

Return the result in **any order**.

---

### 📊 Example  

**Input**

| tweet_id | content                           |
|----------|------------------------------------|
| 1        | Let us Code                        |
| 2        | More than fifteen chars are here!  |

**Output**

| tweet_id |
|----------|
| 2        |

---

### ✅ SQL Query with Explanation  

```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;
```

Let’s break down the logic:

1. SELECT tweet_id:<br>
We want to return just the tweet_id of tweets that violate the character limit.<br>

2. FROM Tweets<br>
This tells SQL to read from the Tweets table that holds all the tweet records.<br>

3. WHERE LENGTH(content) <br>
We apply a condition that filters only those tweets where the length of the content exceeds 15 characters.
The LENGTH() function calculates the number of characters in the string.
If it’s greater than 15, that tweet is considered invalid and included in the result.

