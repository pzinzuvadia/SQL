
# LeetCode Problem: Customer Who Visited but Did Not Make Any Transactions

## Problem Statement

[**Customer Who Visited but Did Not Make Any Transactions**](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)

**Description:**

Given two tables, `Visits` and `Transactions`, write a SQL query to find all the customers who visited but did not make any transactions. 

### Table: `Visits`

| Column Name   | Type    |
|---------------|---------|
| visit_id      | int     |
| customer_id   | int     |
| visit_date    | date    |

`visit_id` is the primary key for this table. This table contains information about customer visits.

### Table: `Transactions`

| Column Name     | Type    |
|-----------------|---------|
| transaction_id  | int     |
| visit_id        | int     |
| amount          | int     |

`transaction_id` is the primary key for this table. This table contains information about transactions made during customer visits.

### Task:
You need to find the customers who visited the store but did not make any transactions during their visits. Return the customer ID and the count of visits where they made no transactions.

## Sample Input:

### Visits Table:

| visit_id | customer_id | visit_date |
|----------|-------------|------------|
| 1        | 101         | 2023-01-01 |
| 2        | 102         | 2023-01-02 |
| 3        | 101         | 2023-01-03 |
| 4        | 103         | 2023-01-04 |

### Transactions Table:

| transaction_id | visit_id | amount |
|----------------|----------|--------|
| 1              | 1        | 200    |
| 2              | 3        | 100    |

## Expected Output:

| customer_id | count_no_trans |
|-------------|----------------|
| 102         | 1              |
| 103         | 1              |

### Explanation:
- Customer 101 made transactions during both visits, so they are not included in the result.
- Customer 102 visited the store once but didn't make any transactions, so they are included with a visit count of 1.
- Customer 103 visited the store but didn't make any transactions, so they are also included with a visit count of 1.

## SQL Solution:

```sql
SELECT v.customer_id, COUNT(v.visit_id) AS count_no_trans
FROM visits v
LEFT JOIN transactions t
ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;
```

## Solution Explanation:

1. **LEFT JOIN**: We perform a `LEFT JOIN` between the `Visits` table and the `Transactions` table. The `LEFT JOIN` ensures that all rows from the `Visits` table are included, even if there are no matching rows in the `Transactions` table.
   
2. **WHERE t.transaction_id IS NULL**: After the `LEFT JOIN`, we filter for rows where the `transaction_id` is `NULL`. This indicates that the visit had no corresponding transaction.

3. **GROUP BY v.customer_id**: We group the results by the `customer_id` to get the number of visits where no transactions were made for each customer.

4. **COUNT(v.visit_id)**: For each group, we count the number of visits where no transactions occurred.

## Conclusion:

This query effectively identifies customers who visited the store but did not complete any transactions, and it provides the number of such visits per customer.
