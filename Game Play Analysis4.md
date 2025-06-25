# 22. Game Play Analysis IV

**Difficulty:** Medium  
**Tags:** Aggregate Functions, Grouping, Joins, Date Arithmetic

---

## Problem Description

Given a table `Activity`, which records players logging in and the number of games they played:

### Table: `Activity`
| Column Name  | Type    | Description                                    |
|--------------|---------|------------------------------------------------|
| `player_id`  | int     | ID of the player                               |
| `device_id`  | int     | ID of the device used                          |
| `event_date` | date    | Date the player logged in                      |
| `games_played` | int   | Number of games played during that session     |

(`player_id`, `event_date`) is the **primary key**.

---

## 🎯 Objective

Calculate the **fraction of players** who logged in again **on the day immediately after** their **first login** date.

- Round the result to **2 decimal places**.
- The output should contain a single column: `fraction`.

---

## Example Input

**Activity Table:**
| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

**Output:**
| fraction |
|----------|
| 0.33     |

---


## SQL Solution

```sql
SELECT 
    ROUND(
        COUNT(DISTINCT a2.player_id) / COUNT(DISTINCT a1.player_id), 
        2
    ) AS fraction
FROM 
    (SELECT 
         player_id, 
         MIN(event_date) AS first_login
     FROM 
         Activity 
     GROUP BY 
         player_id  ) a1
LEFT JOIN 
    Activity a2 
ON 
    a1.player_id = a2.player_id 
    AND a2.event_date = DATE_ADD(a1.first_login, INTERVAL 1 DAY);
```

## Explanation

1. Subquery a1: Finds each player's first login date using ```MIN(event_date)```.
2. Join: Match each player with a row where they logged in on the next day ```(first_login + 1 day)```.
3. Numerator: ```COUNT(DISTINCT a2.player_id)``` counts players who logged in the next day.
4. Denominator: ```COUNT(DISTINCT a1.player_id)``` gives the total number of players.
5. Final Result: Divide and round to 2 decimal places.
---
