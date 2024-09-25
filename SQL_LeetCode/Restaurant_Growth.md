
# Restaurant Growth

## Question

Write an SQL query to find the total revenue of the restaurant for every 7 days, as well as the average revenue per day over these 7 days.

Return the result table with `visited_on`, `amount`, and `average_amount`.

### Example:

**Customer Table:**

| visited_on | amount |
|------------|--------|
| 2021-01-01 | 100    |
| 2021-01-02 | 200    |
| 2021-01-03 | 300    |
| 2021-01-04 | 400    |
| 2021-01-05 | 500    |
| 2021-01-06 | 600    |
| 2021-01-07 | 700    |
| 2021-01-08 | 800    |

**Output:**

| visited_on | amount | average_amount |
|------------|--------|----------------|
| 2021-01-07 | 2800   | 400.00         |
| 2021-01-08 | 3500   | 500.00         |

### Explanation:

- From 2021-01-01 to 2021-01-07, the total revenue is 2800 and the average daily revenue is 400.00.
- From 2021-01-02 to 2021-01-08, the total revenue is 3500 and the average daily revenue is 500.00.

---

## Solution

```sql
WITH cte1 AS (
    SELECT visited_on, SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW) AS amount, 
           RANK() OVER(ORDER BY visited_on) AS rnk
    FROM (
        SELECT visited_on, SUM(amount) AS amount
        FROM customer
        GROUP BY visited_on
    ) x
)

SELECT visited_on, amount, ROUND(amount / 7, 2) AS average_amount
FROM cte1
WHERE rnk >= 7;
```

## Solution Explanation

- The inner query groups the customer data by `visited_on` to calculate the total amount of revenue for each day.
- The outer query uses the `SUM` window function to calculate the total revenue for each 7-day window, ranging from 6 days before to the current day.
- The `RANK` function is used to ensure that only rows with at least 7 days of data are included in the final result.
- The final query selects the total revenue (`amount`) and calculates the average daily revenue (`average_amount`) over the 7-day period.
