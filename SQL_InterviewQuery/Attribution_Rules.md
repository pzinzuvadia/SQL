
# Attribution Rules

## Question

You are given two tables: `user_sessions` containing `session_id` and `user_id`, and `attribution` containing `session_id` and `channel` (such as 'Facebook', 'Google', etc.). Write an SQL query to determine the attribution of each user based on their sessions:

- If a user has any session attributed to 'Facebook' or 'Google', their attribution is 'paid'.
- Otherwise, their attribution is 'organic'.

Return the result with `attribute` ('paid' or 'organic') and `user_id`.

### Example:

**User Sessions Table:**

| session_id | user_id |
|------------|---------|
| 1          | 101     |
| 2          | 102     |
| 3          | 103     |

**Attribution Table:**

| session_id | channel   |
|------------|-----------|
| 1          | Facebook  |
| 2          | Organic   |
| 3          | Google    |

**Output:**

| attribute | user_id |
|-----------|---------|
| paid      | 101     |
| paid      | 103     |
| organic   | 102     |

---

## Solution

```sql
WITH cte1 AS (
    SELECT us.user_id
    FROM user_sessions us
    LEFT JOIN attribution a ON us.session_id = a.session_id
    WHERE a.channel IN ('Facebook', 'Google')
)

SELECT CASE 
           WHEN y.user_id IN (SELECT user_id FROM cte1) THEN 'paid' 
           ELSE 'organic' 
       END AS attribute, 
       y.user_id
FROM (SELECT DISTINCT user_id FROM user_sessions) y;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` selects all users who have sessions with a channel labeled 'Facebook' or 'Google'.
- The main query then assigns each user an attribute based on whether they are present in the `cte1` (indicating 'paid') or not (indicating 'organic').
