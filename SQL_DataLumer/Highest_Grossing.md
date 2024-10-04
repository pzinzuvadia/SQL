
# SQL Highest Grossing

## Question

You are given a table `product_spend` that contains `category`, `product`, `spend`, and `transaction_date`. Write an SQL query to find the top two highest-grossing products (based on total spend) for each category in the year 2022.

Return the result with `category`, `product`, and `total_spend`, ordered by `total_spend` in descending order for each category.

### Example:

**Product Spend Table:**

| category | product  | spend | transaction_date |
|----------|----------|-------|------------------|
| A        | Product1 | 100   | 2022-01-01       |
| A        | Product2 | 150   | 2022-01-01       |
| B        | Product3 | 200   | 2022-01-01       |

**Output:**

| category | product  | total_spend |
|----------|----------|-------------|
| A        | Product2 | 150         |
| A        | Product1 | 100         |
| B        | Product3 | 200         |

---

## Solution

```sql
WITH cte1 AS (
    SELECT category, product, total_spend, 
           DENSE_RANK() OVER (PARTITION BY category ORDER BY total_spend DESC) AS rnk
    FROM (
        SELECT category, product, SUM(spend) AS total_spend
        FROM product_spend
        WHERE EXTRACT(YEAR FROM transaction_date) = '2022'
        GROUP BY category, product
    ) x
)

SELECT category, product, total_spend
FROM cte1
WHERE rnk <= 2;
```

## Solution Explanation

- The inner query calculates the total spend for each product in each category in 2022 by grouping the data by `category` and `product`.
- The outer query uses the `DENSE_RANK()` window function to rank the products by their total spend within each category.
- The main query filters the result to include only the top two products (ranked 1 or 2) for each category.
