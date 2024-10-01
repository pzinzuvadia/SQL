
# Second Day Confirmation

## Question

You are given two tables: `emails` and `texts`. The `emails` table contains `user_id`, `email_id`, and `signup_date`, and the `texts` table contains `email_id`, `signup_action`, and `action_date`.

Write an SQL query to find the `user_id` of all users who confirmed their signup exactly one day after signing up.

Return the result as a list of `user_id`.

### Example:

**Emails Table:**

| user_id | email_id | signup_date |
|---------|----------|-------------|
| 1       | 1001     | 2022-01-01  |
| 2       | 1002     | 2022-01-02  |

**Texts Table:**

| email_id | signup_action | action_date |
|----------|---------------|-------------|
| 1001     | Confirmed      | 2022-01-02  |
| 1002     | Confirmed      | 2022-01-04  |

**Output:**

| user_id |
|---------|
| 1       |

### Explanation:

- User 1 confirmed their signup exactly one day after signing up (2022-01-01 to 2022-01-02).
- User 2 confirmed their signup two days after signing up, so they are not included in the result.

---

## Solution

```sql
SELECT e.user_id
FROM emails e  
JOIN texts t  
ON e.email_id = t.email_id
WHERE t.signup_action = 'Confirmed' 
  AND EXTRACT(DAY FROM (t.action_date - e.signup_date)) = 1;
```

## Solution Explanation

- The query joins the `emails` and `texts` tables on `email_id` to match the signup data with the corresponding confirmation action.
- It filters for rows where the `signup_action` is 'Confirmed' and where the difference between `action_date` and `signup_date` is exactly one day.
- The `EXTRACT(DAY FROM ...)` function calculates the difference in days between the signup and confirmation dates.
