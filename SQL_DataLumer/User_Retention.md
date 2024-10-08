
# User Retention

## Question

You are given a table `user_actions` that contains information about user interactions, including `user_id`, `event_id`, `event_date`, and `event_type`. Write an SQL query to calculate the number of users who were active in both June and July 2022. Active users are defined as those who performed at least one of the following actions: 'sign-in', 'like', or 'comment'.

Return the result with `mth` (month) and `monthly_active_users` (the number of users who were active in both months).

### Example:

**User Actions Table:**

| user_id | event_id | event_date | event_type |
|---------|----------|------------|------------|
| 1       | 1001     | 2022-06-01 | sign-in    |
| 1       | 1002     | 2022-07-01 | comment    |
| 2       | 1003     | 2022-06-01 | like       |
| 3       | 1004     | 2022-07-01 | sign-in    |

**Output:**

| mth | monthly_active_users |
|-----|----------------------|
| 7   | 1                    |

---

## Solution

```sql
WITH cte1 AS (
    SELECT user_id, event_id, event_date, 
           EXTRACT(month FROM event_date) AS mth
    FROM user_actions
    WHERE event_type IN ('sign-in', 'like', 'comment') 
      AND EXTRACT(year FROM event_date) = '2022' 
      AND EXTRACT(month FROM event_date) IN ('6', '7')
)

SELECT mth, COUNT(DISTINCT user_id) AS monthly_active_users
FROM (
    SELECT user_id, mth, 
           DENSE_RANK() OVER (PARTITION BY user_id ORDER BY mth) AS rnk
    FROM cte1
) x 
WHERE rnk = 2
GROUP BY mth;
```

## Solution Explanation

- The query first filters the `user_actions` table to include only actions that occurred in June and July 2022 for the specified event types ('sign-in', 'like', 'comment').
- The `DENSE_RANK()` function is used to rank the months for each user.
- The main query counts the number of users who have a rank of 2, meaning they were active in both months.
- The result is grouped by the month (`mth`), and the number of active users is returned.
