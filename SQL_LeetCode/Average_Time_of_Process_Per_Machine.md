
# LeetCode Problem: Average Time of Process Per Machine

## Problem Statement

[**Average Time of Process Per Machine**](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)

**Description:**

Given a table `Activity`, write a SQL query to find the average processing time for each machine. 

### Table: `Activity`

| Column Name    | Type    |
|----------------|---------|
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |

`machine_id` is the primary key for this table. Each row of this table records the start or end of a process on a machine. There are only two types of activities in this table:
- `'start'`: process starts at the corresponding timestamp for the machine.
- `'end'`: process ends at the corresponding timestamp for the machine.

### Task:
For each machine, calculate the average processing time of all its processes, rounded to 3 decimal places.

## Sample Input:

### Activity Table:

| machine_id | process_id | activity_type | timestamp |
|------------|------------|----------------|-----------|
| 1          | 1001       | 'start'        | 0.000     |
| 1          | 1001       | 'end'          | 3.000     |
| 1          | 1002       | 'start'        | 5.000     |
| 1          | 1002       | 'end'          | 7.000     |
| 2          | 1003       | 'start'        | 2.000     |
| 2          | 1003       | 'end'          | 5.000     |

## Expected Output:

| machine_id | processing_time |
|------------|-----------------|
| 1          | 2.500           |
| 2          | 3.000           |

### Explanation:
- Machine 1 processed two processes: 
  - The first took 3.000 - 0.000 = 3.000 units of time.
  - The second took 7.000 - 5.000 = 2.000 units of time.
  The average time for machine 1 is (3.000 + 2.000) / 2 = 2.500.

- Machine 2 processed one process which took 5.000 - 2.000 = 3.000 units of time.

## SQL Solution:

```sql
SELECT machine_id, ROUND(AVG(time_taken), 3) AS processing_time
FROM (
    SELECT end.machine_id AS machine_id, end.process_id AS process_id, 
           ROUND(end.end_time - start.start_time, 3) AS time_taken
    FROM (
        SELECT machine_id, process_id, timestamp AS end_time
        FROM activity
        WHERE activity_type = 'end'
        GROUP BY machine_id, process_id
    ) end
    JOIN (
        SELECT machine_id, process_id, timestamp AS start_time
        FROM activity
        WHERE activity_type = 'start'
        GROUP BY machine_id, process_id
    ) start
    ON end.machine_id = start.machine_id AND end.process_id = start.process_id
) x
GROUP BY machine_id;
```

## Solution Explanation:

1. **Subquery to Extract Start and End Times**: 
   - First, we extract the `end_time` from the `activity` table where `activity_type = 'end'`, and the `start_time` where `activity_type = 'start'`. These subqueries select and group by `machine_id` and `process_id`.

2. **Calculate Time Taken**: 
   - After joining the `start` and `end` subqueries on `machine_id` and `process_id`, we calculate the time taken for each process by subtracting the `start_time` from the `end_time`.

3. **Calculate Average Processing Time**:
   - Finally, we calculate the average processing time for each `machine_id` by averaging the `time_taken` values and rounding to 3 decimal places.

## Conclusion:

This query efficiently calculates the average processing time for each machine by first joining the start and end times of processes, calculating their durations, and then averaging those durations per machine.
