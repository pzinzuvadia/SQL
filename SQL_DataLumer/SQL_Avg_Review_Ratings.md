
# SQL Average Review Ratings

## Question

You are given a table `reviews` that contains `product_id`, `stars` (rating), and `submit_date`. Write an SQL query to find the average rating for each product in each month.

Return the result with the `mth` (month), `product_id` (renamed as `product`), and `avg_stars` (average rating) rounded to two decimal places. The result should be ordered by `mth` and then by `product_id`.

### Example:

**Reviews Table:**

| product_id | stars | submit_date |
|------------|-------|-------------|
| 1          | 5     | 2022-01-15  |
| 1          | 4     | 2022-01-20  |
| 2          | 3     | 2022-02-01  |
| 1          | 4     | 2022-02-05  |

**Output:**

| mth  | product | avg_stars |
|------|---------|-----------|
| 1    | 1       | 4.50      |
| 2    | 1       | 4.00      |
| 2    | 2       | 3.00      |

### Explanation:

- In January, product 1 has an average rating of 4.5.
- In February, product 1 has an average rating of 4.0, and product 2 has an average rating of 3.0.

---

## Solution

```sql
SELECT mth, product_id AS product, ROUND(AVG(stars), 2) AS avg_stars
FROM (
    SELECT EXTRACT(MONTH FROM submit_date) AS mth, product_id, stars
    FROM reviews
) x 
GROUP BY mth, product_id
ORDER BY mth, product;
```

## Solution Explanation

- The inner query extracts the month from the `submit_date` and groups the data by `product_id` and month (`mth`).
- The outer query calculates the average rating for each product in each month, rounding it to two decimal places.
- The result is ordered by `mth` and then by `product`.
