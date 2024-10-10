
# Signup Confirmation Rate

## Question

You are given two tables: `emails` and `texts`. The `emails` table contains `email_id` and `email_address`, and the `texts` table contains `email_id` and `signup_action` (either 'Confirmed' or another status). Write an SQL query to calculate the signup confirmation rate as a percentage, rounded to two decimal places.

Return the result as the confirmation rate in percentage.

### Example:

**Emails Table:**

| email_id | email_address     |
|----------|-------------------|
| 1        | user1@example.com |
| 2        | user2@example.com |

**Texts Table:**

| email_id | signup_action |
|----------|---------------|
| 1        | Confirmed      |
| 2        | Pending        |

**Output:**

| confirmation_rate |
|-------------------|
| 50.00             |

---

## Solution

```sql
SELECT ROUND(100.0 * COUNT(CASE WHEN signup_action = 'Confirmed' THEN 1 END) / 
             ((SELECT COUNT(DISTINCT email_id) FROM emails) * 100.0), 2) AS confirmation_rate
FROM texts;
```

## Solution Explanation

- The query calculates the total number of confirmed signups by counting the rows in the `texts` table where the `signup_action` is 'Confirmed'.
- It divides the count of confirmed signups by the total number of distinct `email_id` values from the `emails` table, and multiplies the result by 100 to get the percentage.
- The result is rounded to two decimal places using the `ROUND()` function and returned as the confirmation rate.
