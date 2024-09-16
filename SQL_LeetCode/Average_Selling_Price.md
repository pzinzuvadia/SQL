
# Average Selling Price

## Question

Write an SQL query to find the **average selling price** for each product.

The average selling price of a product is calculated by dividing the total revenue by the total number of units sold. Total revenue for a product is the sum of `price * units` of each transaction.

Return the result table in **any order**.

### Example:

**Prices Table:**

| product_id | start_date | end_date   | price |
|------------|------------|------------|-------|
| 1          | 2020-02-17 | 2020-02-28 | 5     |
| 1          | 2020-03-01 | 2020-03-22 | 20    |
| 2          | 2020-02-01 | 2020-02-20 | 15    |

**UnitsSold Table:**

| product_id | purchase_date | units |
|------------|----------------|-------|
| 1          | 2020-02-25     | 100   |
| 1          | 2020-03-01     | 15    |
| 2          | 2020-02-10     | 1     |
| 2          | 2020-02-21     | 10    |

**Output:**

| product_id | average_price |
|------------|---------------|
| 1          | 6.67          |
| 2          | 15.00         |

### Explanation:

- For product 1, the total revenue is (5*100 + 20*15) = 500 + 300 = 800, and the total units sold is 100 + 15 = 115. The average selling price is 800 / 115 = 6.67.
- For product 2, the total revenue is (15*1) = 15, and the total units sold is 1. The average selling price is 15 / 1 = 15.00.

---

## Solution

```sql
SELECT x.product_id, COALESCE(ROUND(SUM(x.price * x.units) / SUM(units), 2), 0) AS average_price
FROM 
(
    SELECT p.product_id, p.price, u.units
    FROM prices p
    LEFT JOIN unitssold u
    ON p.product_id = u.product_id AND u.purchase_date BETWEEN p.start_date AND p.end_date
) x
GROUP BY x.product_id;
```

## Solution Explanation

- We use a **left join** to combine the `prices` and `unitssold` tables based on `product_id`, ensuring that the units sold are within the valid price range (`u.purchase_date BETWEEN p.start_date AND p.end_date`).
- The query multiplies the price by the units sold to calculate the total revenue for each product.
- The `SUM` function calculates the total revenue and the total number of units sold for each product.
- The average price is computed by dividing the total revenue by the total units sold, and the `ROUND` function is used to round the result to two decimal places.
