
# Confirmation Rate

## Question

Write an SQL query to find the **confirmation rate** for each user.

The confirmation rate of a user is defined as the ratio of the number of "confirmed" actions to the total number of actions for that user in the `confirmations` table.

The confirmation rate should be **rounded to two decimal places**.

Return the result table in **any order**.

### Example:

**Signups Table:**

| user_id | time       |
|---------|------------|
| 1       | 2020-10-01 |
| 2       | 2020-10-02 |
| 3       | 2020-10-03 |
| 4       | 2020-10-04 |
| 5       | 2020-10-05 |

**Confirmations Table:**

| user_id | action     |
|---------|------------|
| 1       | 'confirmed'|
| 1       | 'failed'   |
| 1       | 'failed'   |
| 2       | 'confirmed'|
| 2       | 'confirmed'|
| 3       | 'failed'   |
| 3       | 'confirmed'|
| 4       | 'failed'   |
| 5       | 'confirmed'|
| 5       | 'confirmed'|

**Output:**

| user_id | confirmation_rate |
|---------|-------------------|
| 1       | 0.33              |
| 2       | 1.00              |
| 3       | 0.50              |
| 4       | 0.00              |
| 5       | 1.00              |

### Explanation:

- User 1 had 1 "confirmed" action out of 3 actions in total, so their confirmation rate is 1/3 = 0.33.
- User 2 had 2 "confirmed" actions out of 2 actions in total, so their confirmation rate is 2/2 = 1.00.
- User 3 had 1 "confirmed" action out of 2 actions in total, so their confirmation rate is 1/2 = 0.50.
- User 4 had 0 "confirmed" actions out of 1 action in total, so their confirmation rate is 0/1 = 0.00.
- User 5 had 2 "confirmed" actions out of 2 actions in total, so their confirmation rate is 2/2 = 1.00.

---

## Solution

```sql
SELECT x.user_id, ROUND(COALESCE((c_cnt/(SELECT COUNT(action) FROM confirmations WHERE user_id = x.user_id)), 0), 2) AS confirmation_rate
FROM
(
    SELECT s.user_id, COUNT(CASE WHEN c.action = 'confirmed' THEN 1 END) AS c_cnt
    FROM signups s
    LEFT JOIN confirmations c
    ON s.user_id = c.user_id 
    GROUP BY s.user_id
) x
ORDER BY confirmation_rate;
```

## Solution Explanation

- We first use a **left join** to join the `signups` and `confirmations` tables based on `user_id`, ensuring that all users from `signups` are included even if they have no confirmations.
- The `CASE` statement counts the number of "confirmed" actions for each user.
- The outer query calculates the confirmation rate by dividing the count of "confirmed" actions by the total number of actions for each user in the `confirmations` table.
- The `ROUND` function is used to round the result to two decimal places.
