
# ATM Robbery

## Question

You are given a table `bank_transactions` that contains `user_id` and `created_at` (the timestamp of the transaction). Write an SQL query to identify users who made two transactions exactly 10 seconds apart, indicating a possible ATM robbery attempt.

Return the result with `user_id` for any user who meets this condition.

### Example:

**Bank Transactions Table:**

| user_id | created_at         |
|---------|---------------------|
| 101     | 2023-01-01 10:00:00 |
| 101     | 2023-01-01 10:00:10 |
| 102     | 2023-01-01 10:05:00 |
| 103     | 2023-01-01 10:10:00 |

**Output:**

| user_id |
|---------|
| 101     |

---

## Solution

```sql
SELECT user_id
FROM (
    SELECT user_id, created_at, LEAD(created_at) OVER (ORDER BY created_at) AS next
    FROM bank_transactions
    ORDER BY created_at
) x
WHERE TIMESTAMPDIFF(SECOND, created_at, next) = 10

UNION 

SELECT user_id
FROM (
    SELECT user_id, created_at, LAG(created_at) OVER (ORDER BY created_at) AS next
    FROM bank_transactions
    ORDER BY created_at
) x
WHERE TIMESTAMPDIFF(SECOND, created_at, next) = -10;
```

## Solution Explanation

- The first part of the query identifies any user who made two transactions exactly 10 seconds apart by using the `LEAD` function.
- The second part of the query uses the `LAG` function to check the reverse order, ensuring that both forwards and backwards checks are considered.
- A union operation combines the results, returning the unique `user_id`s who meet the condition of two transactions exactly 10 seconds apart.
