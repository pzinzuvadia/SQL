
# Time Spent on Snaps

## Question

You are given two tables: `activities` and `age_breakdown`. The `activities` table contains `user_id`, `activity_type`, and `time_spent`. The `age_breakdown` table contains `user_id` and `age_bucket`. Write an SQL query to calculate the percentage of time spent on two types of activities ('open' and 'send') for each age group.

Return the result with the `age_bucket`, `send_perc` (percentage of time spent on sending snaps), and `open_perc` (percentage of time spent on opening snaps), rounded to two decimal places.

### Example:

**Activities Table:**

| user_id | activity_type | time_spent |
|---------|---------------|------------|
| 1       | open          | 30         |
| 1       | send          | 70         |
| 2       | open          | 50         |
| 2       | send          | 50         |

**Age Breakdown Table:**

| user_id | age_bucket |
|---------|------------|
| 1       | 18-24      |
| 2       | 25-34      |

**Output:**

| age_bucket | send_perc | open_perc |
|------------|-----------|-----------|
| 18-24      | 70.00     | 30.00     |
| 25-34      | 50.00     | 50.00     |

### Explanation:

- For the age group 18-24, 70% of the time was spent on sending snaps and 30% on opening snaps.
- For the age group 25-34, 50% of the time was spent on sending snaps and 50% on opening snaps.

---

## Solution

```sql
WITH cte1 AS (
    SELECT a.user_id, a.activity_type, a.time_spent, age_bucket
    FROM activities a  
    JOIN age_breakdown ag
    ON a.user_id = ag.user_id
    WHERE a.activity_type IN ('open', 'send')
)

SELECT age_bucket, 
       ROUND((100 * SUM(CASE WHEN activity_type = 'send' THEN time_spent END)) / SUM(time_spent), 2) AS send_perc, 
       ROUND((100.0 * SUM(CASE WHEN activity_type = 'open' THEN time_spent END)) / SUM(time_spent), 2) AS open_perc
FROM cte1 
GROUP BY age_bucket;
```

## Solution Explanation

- The common table expression (CTE) `cte1` joins the `activities` and `age_breakdown` tables and filters the activities to include only 'open' and 'send'.
- The main query calculates the percentage of time spent on each activity type ('send' and 'open') for each age group by summing the time spent on each activity and dividing it by the total time spent.
- The result is rounded to two decimal places and grouped by `age_bucket`.
