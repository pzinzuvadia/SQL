
# Year-Over-Year Growth Rate

## Question

You are given a table `user_transactions` that contains `product_id`, `transaction_date`, and `spend` (the amount spent on each transaction). Write an SQL query to calculate the year-over-year growth rate of spend for each product. The growth rate is calculated as:

\[
	ext{{Growth Rate}} = rac{{	ext{{Current Year Spend}} - 	ext{{Previous Year Spend}}}}{{	ext{{Previous Year Spend}}}} 	imes 100
\]

Return the result with `year`, `product_id`, `curr_year_spend` (current year's spend), `prev_year_spend` (previous year's spend), and the growth rate, rounded to two decimal places.

### Example:

**User Transactions Table:**

| product_id | transaction_date | spend |
|------------|------------------|-------|
| 1          | 2022-01-01       | 1000  |
| 1          | 2021-01-01       | 800   |
| 2          | 2022-01-01       | 1500  |

**Output:**

| year | product_id | curr_year_spend | prev_year_spend | growth_rate |
|------|------------|-----------------|-----------------|-------------|
| 2022 | 1          | 1000            | 800             | 25.00       |
| 2022 | 2          | 1500            | NULL            | NULL        |

---

## Solution

```sql
WITH cte1 AS (
    SELECT product_id, spend, year, 
           DENSE_RANK() OVER (PARTITION BY product_id ORDER BY year DESC) AS rnk, 
           LEAD(spend) OVER (PARTITION BY product_id ORDER BY year DESC) AS prev_spend 
    FROM (
        SELECT product_id, spend, EXTRACT(year FROM transaction_date) AS year
        FROM user_transactions
    ) x
)

SELECT year, product_id, spend AS curr_year_spend, prev_spend AS prev_year_spend, 
       ROUND(100.0 * (spend - prev_spend) / prev_spend, 2) AS growth_rate
FROM cte1
ORDER BY product_id, year, curr_year_spend, prev_year_spend;
```

## Solution Explanation

- The query first calculates the spend for each product by year, and uses the `LEAD()` function to access the previous year's spend for each product.
- The growth rate is calculated using the formula \((	ext{{spend}} - 	ext{{prev\_spend}}) / 	ext{{prev\_spend}} 	imes 100\), and the result is rounded to two decimal places.
- The result is ordered by `product_id`, `year`, `curr_year_spend`, and `prev_year_spend`.
