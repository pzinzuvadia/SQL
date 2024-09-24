
# Product Price at a Given Date

## Question

Write an SQL query to find the price of each product on a given date. If there is no price change on or before the given date, the price is set to 10 by default.

Return the result table in **any order**.

### Example:

**Products Table:**

| product_id | new_price | change_date |
|------------|-----------|-------------|
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-15  |
| 1          | 30        | 2019-08-15  |
| 1          | 40        | 2019-08-16  |
| 2          | 60        | 2019-08-17  |

**Output:**

| product_id | price |
|------------|-------|
| 1          | 40    |
| 2          | 50    |

### Explanation:

- For product 1, the last price change on or before 2019-08-16 was 40, so the price is 40.
- For product 2, the last price change on or before 2019-08-16 was 50, so the price is 50.

---

## Solution

```sql
SELECT p.product_id, COALESCE(
    (SELECT new_price 
     FROM products x 
     WHERE (p.product_id = x.product_id) AND (change_date <= '2019-08-16') 
     ORDER BY change_date DESC LIMIT 1), 10) AS price
FROM (SELECT DISTINCT product_id FROM products) p;
```

## Solution Explanation

- The subquery finds the latest `new_price` for each product on or before the given date ('2019-08-16'). It orders the prices by `change_date` in descending order and limits the result to the most recent price.
- The `COALESCE` function returns the latest price if available, and if not, it defaults to 10.
- The outer query selects all distinct products and applies the subquery to find the price for each product.
