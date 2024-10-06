
# Histogram of Users' Purchases

## Question

You are given a table `user_transactions` that contains `user_id` and `transaction_date`. Write an SQL query to find the number of purchases made by each user on the most recent transaction date.

Return the result with `transaction_date`, `user_id`, and `purchase_count` (the number of purchases made by each user on their most recent transaction date), ordered by `transaction_date` and `user_id`.

### Example:

**User Transactions Table:**

| transaction_date | user_id |
|------------------|---------|
| 2023-07-01       | 1       |
| 2023-07-02       | 1       |
| 2023-07-01       | 2       |

**Output:**

| transaction_date | user_id | purchase_count |
|------------------|---------|----------------|
| 2023-07-02       | 1       | 1              |
| 2023-07-01       | 2       | 1              |

---

## Solution

```sql
WITH cte1 AS (
    SELECT transaction_date, user_id, 
           DENSE_RANK() OVER (PARTITION BY user_id ORDER BY transaction_date DESC) AS rnk  
    FROM user_transactions
)

SELECT transaction_date, user_id, COUNT(*) AS purchase_count
FROM cte1
WHERE rnk = 1
GROUP BY transaction_date, user_id
ORDER BY transaction_date, user_id;
```

## Solution Explanation

- The common table expression (CTE) `cte1` ranks each user's transactions by `transaction_date` in descending order using `DENSE_RANK()`. This assigns a rank of 1 to the most recent transaction for each user.
- The main query filters the result to include only the most recent transaction for each user (`rnk = 1`).
- The result is grouped by `transaction_date` and `user_id` and returns the number of purchases (`purchase_count`) made by each user on their most recent transaction date.
