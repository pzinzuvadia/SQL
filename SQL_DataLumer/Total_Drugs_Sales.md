
# Total Drugs Sales

## Question

You are given a table `pharmacy_sales` that contains `manufacturer` and `total_sales`. Write an SQL query to find the total drug sales for each manufacturer, and display the sales in millions of dollars.

Return the result with the `manufacturer` and `sale` in the format `'$X million'`, where `X` is the total sales rounded to the nearest million. The result should be ordered by `total_sales` in descending order, and then by `manufacturer` in ascending order if there is a tie.

### Example:

**Pharmacy Sales Table:**

| manufacturer | total_sales |
|--------------|-------------|
| Pfizer       | 123456789   |
| Moderna      | 987654321   |
| Pfizer       | 100000000   |

**Output:**

| manufacturer | sale        |
|--------------|-------------|
| Moderna      | $988 million|
| Pfizer       | $223 million|

### Explanation:

- Pfizer's total sales sum up to $223 million.
- Moderna's total sales sum up to $988 million.

---

## Solution

```sql
WITH cte1 AS (
    SELECT manufacturer, SUM(total_sales) AS total_sales
    FROM pharmacy_sales
    GROUP BY manufacturer
)

SELECT manufacturer, CONCAT('$', ROUND(total_sales / 1000000, 0), ' million') AS sale
FROM cte1
ORDER BY total_sales DESC, manufacturer;
```

## Solution Explanation

- The common table expression (CTE) `cte1` groups the `pharmacy_sales` table by `manufacturer` and calculates the total sales for each manufacturer.
- The main query formats the total sales as millions of dollars, rounding to the nearest million, and concatenates the result with the string `'$X million'`.
- The result is ordered by `total_sales` in descending order, and in case of a tie, by `manufacturer` in ascending order.
