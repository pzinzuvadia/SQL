
# Total Utilization Time

## Question

You are given a table `server_utilization` that contains `server_id`, `status_time`, and `session_status` (either 'start' or 'stop'). Write an SQL query to calculate the total uptime of each server, where uptime is defined as the time between the 'start' and 'stop' session statuses.

Return the result as `total_uptime_days`, which represents the total uptime in days, rounded to the nearest whole number.

### Example:

**Server Utilization Table:**

| server_id | status_time       | session_status |
|-----------|-------------------|----------------|
| 1         | 2023-07-01 08:00  | start          |
| 1         | 2023-07-01 16:00  | stop           |
| 2         | 2023-07-01 09:00  | start          |

**Output:**

| total_uptime_days |
|-------------------|
| 1                 |

---

## Solution

```sql
WITH cte1 AS (
    SELECT server_id, status_time
    FROM server_utilization
    WHERE session_status = 'start'
),
  
cte2 AS (
    SELECT server_id, status_time
    FROM server_utilization
    WHERE session_status = 'stop'
)

SELECT ROUND(SUM((EXTRACT(epoch FROM min - status_time) / 3600) / 24), 0) AS total_uptime_days
FROM (
    SELECT c1.server_id, c1.status_time, MIN(c2.status_time)
    FROM cte1 c1
    JOIN cte2 c2
    ON c1.server_id = c2.server_id
    WHERE c2.status_time > c1.status_time
    GROUP BY c1.server_id, c1.status_time
) x;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` filters the `server_utilization` table to include only rows where the session status is 'start'.
- The second CTE `cte2` filters the table for 'stop' session statuses.
- The main query calculates the total uptime for each server by finding the difference between the 'start' and 'stop' times for each session.
- The result is rounded to the nearest whole number and returned as the total uptime in days.
