
# Completed Trades

## Question

You are given two tables: `trades` and `users`. The `trades` table contains `user_id`, `order_id`, and `status`, and the `users` table contains `user_id` and `city`. Write an SQL query to find the top three cities with the highest number of completed trades.

Return the result with the `city` and the total number of `total_orders` of completed trades, ordered in descending order by `total_orders`. If there is a tie, return any cities with the highest trade counts.

### Example:

**Trades Table:**

| user_id | order_id | status    |
|---------|----------|-----------|
| 1       | 1001     | Completed |
| 2       | 1002     | Pending   |
| 1       | 1003     | Completed |
| 3       | 1004     | Completed |

**Users Table:**

| user_id | city   |
|---------|--------|
| 1       | New York|
| 2       | San Francisco|
| 3       | Los Angeles|

**Output:**

| city          | total_orders |
|---------------|--------------|
| New York      | 2            |
| Los Angeles   | 1            |

### Explanation:

- New York has 2 completed trades, and Los Angeles has 1 completed trade.

---

## Solution

```sql
WITH cte1 AS (
    SELECT t.user_id, u.city, COUNT(t.order_id) AS cnt
    FROM trades t  
    JOIN users u   
    ON t.user_id = u.user_id
    WHERE t.status = 'Completed'
    GROUP BY t.user_id, u.city
)

SELECT city, SUM(cnt) AS total_orders
FROM cte1
GROUP BY city
ORDER BY total_orders DESC 
LIMIT 3;
```

## Solution Explanation

- The common table expression (CTE) `cte1` joins the `trades` and `users` tables to count the number of completed trades (`status = 'Completed'`) for each user and their corresponding city.
- The outer query sums the number of completed trades by `city` and orders the result by `total_orders` in descending order, limiting the result to the top 3 cities.
