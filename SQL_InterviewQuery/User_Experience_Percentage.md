
# User Experience Percentage

## Question

You are given a table `user_experiences` that contains `user_id`, `position_name`, and `start_date` for each user's job experiences. Write an SQL query to calculate the percentage of users who transitioned from 'Data Analyst' to 'Data Scientist'.

The result should be returned as a percentage, rounded to two decimal places if necessary.

### Example:

**User Experiences Table:**

| user_id | position_name | start_date |
|---------|---------------|------------|
| 1       | Data Analyst  | 2020-01-01 |
| 1       | Data Scientist| 2021-01-01 |
| 2       | Data Analyst  | 2020-02-01 |
| 2       | Engineer      | 2021-02-01 |

**Output:**

| percentage |
|------------|
| 0.5        |

---

## Solution

```sql
WITH cte1 AS (
    SELECT position_name, start_date, user_id, 
           LEAD(position_name) OVER (PARTITION BY user_id ORDER BY start_date) AS next_role
    FROM user_experiences
)

SELECT COUNT(*) / (SELECT COUNT(DISTINCT user_id) FROM user_experiences) AS percentage
FROM (
    SELECT DISTINCT user_id
    FROM cte1
    WHERE position_name = 'Data Analyst' AND next_role = 'Data Scientist'
) x;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` selects each user's `position_name`, `start_date`, and the `next_role` they transitioned to, using the `LEAD()` function to identify the subsequent position in each user's job history.
- The main query then counts the number of unique users who transitioned from 'Data Analyst' to 'Data Scientist' and divides it by the total number of unique users.
- The result is returned as the percentage of users who made this career transition.
