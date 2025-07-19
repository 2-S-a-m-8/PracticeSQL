### 10. Average Time of Process per Machine  
**Difficulty:** Easy  
**Platform:** LeetCode  
**Tags:** SQL, Aggregation, Self-Join  

---

### Problem Statement

You are given a table `Activity` with the following schema:

| Column Name    | Type    |
|----------------|---------|
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum('start','end') |
| timestamp      | float   |

Each row in this table represents either the start or end of a process being executed on a machine. Each process is uniquely identified by a combination of `machine_id` and `process_id`.  
It is guaranteed that for every `(machine_id, process_id)` pair:
- There will be one row where `activity_type = 'start'`
- And one row where `activity_type = 'end'`
- The `start` timestamp will always occur before the `end` timestamp.

**Task:**  
For each `machine_id`, compute the average time the machine takes to complete a process.  
This is calculated as the mean of `(end.timestamp - start.timestamp)` over all processes executed by that machine.  
The result should include two columns: `machine_id` and `processing_time`, with the processing time rounded to three decimal places.

---

### Sample Input

| machine_id | process_id | activity_type | timestamp |
|------------|------------|----------------|-----------|
| 0          | 0          | start          | 0.712     |
| 0          | 0          | end            | 1.520     |
| 0          | 1          | start          | 3.140     |
| 0          | 1          | end            | 4.120     |
| 1          | 0          | start          | 0.550     |
| 1          | 0          | end            | 1.550     |
| 1          | 1          | start          | 0.430     |
| 1          | 1          | end            | 1.420     |
| 2          | 0          | start          | 4.100     |
| 2          | 0          | end            | 4.512     |
| 2          | 1          | start          | 2.500     |
| 2          | 1          | end            | 5.000     |

---

### Expected Output

| machine_id | processing_time |
|------------|-----------------|
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |

---

### Explanation

Each machine executes multiple processes, and for each process there are exactly two entries: one for the start and one for the end.  
The time taken to complete a single process is computed as: processing_time = end.timestamp - start.timestamp

To determine the average processing time for each machine:
1. Join the table to itself, pairing each `start` activity with its corresponding `end` activity using the `machine_id` and `process_id`.
2. Calculate the time difference for each matched process.
3. Compute the average of these time differences for each `machine_id`.
4. Round the result to three decimal places.

For example, for `machine_id = 0`, the two processes have durations:
- Process 0: 1.520 - 0.712 = 0.808
- Process 1: 4.120 - 3.140 = 0.980  
Average = (0.808 + 0.980) / 2 = 0.894

---

### Final SQL Query

```sql
SELECT 
  a.machine_id,
  ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
FROM Activity a 
JOIN Activity b 
  ON a.machine_id = b.machine_id 
  AND a.process_id = b.process_id
  AND a.activity_type = 'start'
  AND b.activity_type = 'end'
GROUP BY a.machine_id;
```

