
# Repeated Payments

## Question

You are given a table `transactions` that contains `merchant_id`, `credit_card_id`, `amount`, and `transaction_timestamp`. Write an SQL query to find the total number of repeated payments. A repeated payment is defined as multiple transactions with the same `merchant_id`, `credit_card_id`, and `amount`, occurring within 10 minutes of each other.

Return the total number of repeated payments.

### Example:

**Transactions Table:**

| merchant_id | credit_card_id | amount | transaction_timestamp  |
|-------------|----------------|--------|------------------------|
| 1           | 1001           | 100    | 2023-07-01 08:00:00    |
| 1           | 1001           | 100    | 2023-07-01 08:05:00    |
| 2           | 1002           | 200    | 2023-07-01 09:00:00    |

**Output:**

| repeated_payments |
|-------------------|
| 1                 |

---

## Solution

```sql
WITH cte1 AS (
    SELECT merchant_id, credit_card_id, amount
    FROM (
        SELECT merchant_id, credit_card_id, amount, transaction_timestamp, 
               LEAD(transaction_timestamp) OVER (ORDER BY merchant_id, credit_card_id, amount, transaction_timestamp) AS next_stamp
        FROM transactions
    ) x
    WHERE EXTRACT(minute FROM (next_stamp - transaction_timestamp)) <= 10
)

SELECT SUM(cnt)
FROM (
    SELECT COUNT(*) - 1 AS cnt
    FROM cte1
    GROUP BY merchant_id, credit_card_id, amount
    HAVING COUNT(*) > 1
) x;
```

## Solution Explanation

- The query uses a common table expression (CTE) `cte1` to identify repeated transactions by comparing the `transaction_timestamp` with the next transaction timestamp using the `LEAD()` function.
- The main query counts the number of repeated transactions that occur within 10 minutes and groups the results by `merchant_id`, `credit_card_id`, and `amount`.
- The final result sums the number of repeated transactions.
