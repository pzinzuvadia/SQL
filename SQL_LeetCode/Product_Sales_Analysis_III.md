
# Product Sales Analysis III

## Question

Write an SQL query to find the quantity and price for each product’s **first year** of sales.

Return the result table in **any order**.

### Example:

**Sales Table:**

| product_id | year | quantity | price |
|------------|------|----------|-------|
| 1          | 2020 | 100      | 10    |
| 1          | 2021 | 150      | 15    |
| 2          | 2020 | 200      | 20    |
| 2          | 2021 | 300      | 25    |

**Output:**

| product_id | first_year | quantity | price |
|------------|------------|----------|-------|
| 1          | 2020       | 100      | 10    |
| 2          | 2020       | 200      | 20    |

### Explanation:

- For product 1, the first year is 2020, with a quantity of 100 and a price of 10.
- For product 2, the first year is 2020, with a quantity of 200 and a price of 20.

---

## Solution

```sql
WITH cte1 AS (
    SELECT product_id, MIN(year) AS first_year
    FROM sales
    GROUP BY product_id
)

SELECT s.product_id, c.first_year, s.quantity, s.price
FROM sales s
JOIN cte1 c
ON s.product_id = c.product_id AND s.year = c.first_year;
```

## Solution Explanation

- The common table expression (CTE), `cte1`, finds the **first year** of sales for each product by using `MIN(year)` and grouping by `product_id`.
- The main query joins the `sales` table with the CTE `cte1` to get the quantity and price for each product in its first year of sales.
