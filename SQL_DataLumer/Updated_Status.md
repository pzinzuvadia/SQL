
# Updated Status

## Question

You are given two tables: `advertiser` and `daily_pay`. The `advertiser` table contains `user_id` and `status` (which can be 'NEW', 'EXISTING', 'CHURN', or 'RESURRECT'), and the `daily_pay` table contains `user_id` and `paid` (which indicates whether a payment was made). Write an SQL query to update the status of each user based on their payment history.

Return the result with `user_id` and `new_status`, where:
- If the `status` is 'NEW' and the user has made a payment, their new status is 'EXISTING'.
- If the `status` is 'NEW' and the user has not made a payment, their new status is 'CHURN'.
- If the `status` is 'EXISTING' and the user has made a payment, their new status is 'EXISTING'.
- If the `status` is 'EXISTING' and the user has not made a payment, their new status is 'CHURN'.
- If the `status` is 'CHURN' and the user has made a payment, their new status is 'RESURRECT'.
- If the `status` is 'CHURN' and the user has not made a payment, their new status remains 'CHURN'.
- If the `status` is 'RESURRECT' and the user has made a payment, their new status is 'EXISTING'.
- If the `status` is 'RESURRECT' and the user has not made a payment, their new status is 'CHURN'.

### Example:

**Advertiser Table:**

| user_id | status   |
|---------|----------|
| 1       | NEW      |
| 2       | EXISTING |
| 3       | CHURN    |

**Daily Pay Table:**

| user_id | paid  |
|---------|-------|
| 1       | 100   |
| 3       | NULL  |

**Output:**

| user_id | new_status |
|---------|------------|
| 1       | EXISTING   |
| 2       | CHURN      |
| 3       | CHURN      |

---

## Solution

```sql
WITH cte1 AS (
    SELECT a.user_id, a.status, dp.paid
    FROM advertiser a  
    LEFT JOIN daily_pay dp ON a.user_id = dp.user_id
    UNION
    SELECT dp.user_id, a.status, dp.paid
    FROM advertiser a  
    RIGHT JOIN daily_pay dp ON a.user_id = dp.user_id
)

SELECT user_id, 
       CASE 
           WHEN status = 'NEW' AND paid IS NOT NULL THEN 'EXISTING'
           WHEN status = 'NEW' AND paid IS NULL THEN 'CHURN'
           WHEN status = 'EXISTING' AND paid IS NOT NULL THEN 'EXISTING'
           WHEN status = 'EXISTING' AND paid IS NULL THEN 'CHURN'
           WHEN status = 'CHURN' AND paid IS NOT NULL THEN 'RESURRECT'
           WHEN status = 'CHURN' AND paid IS NULL THEN 'CHURN'
           WHEN status = 'RESURRECT' AND paid IS NOT NULL THEN 'EXISTING'
           WHEN status = 'RESURRECT' AND paid IS NULL THEN 'CHURN'
           ELSE 'NEW' 
       END AS new_status
FROM cte1
ORDER BY user_id;
```

## Solution Explanation

- The query first uses a common table expression (CTE) `cte1` to join the `advertiser` and `daily_pay` tables, ensuring all users are included.
- The main query applies a `CASE` statement to update each user's status based on the rules provided, considering whether or not a payment has been made.
