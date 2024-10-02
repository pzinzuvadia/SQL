
# Non-Profitable Drugs

## Question

You are given a table `pharmacy_sales` that contains `manufacturer`, `drug`, `cogs` (cost of goods sold), and `total_sales`. Write an SQL query to find the manufacturers with non-profitable drugs, where the total sales are less than the cost of goods sold (`total_sales - cogs < 0`).

Return the result with the `manufacturer`, the number of non-profitable drugs (`drug_count`), and the total loss (`total_loss`), ordered by `total_loss` in descending order.

### Example:

**Pharmacy Sales Table:**

| manufacturer | drug     | cogs | total_sales |
|--------------|----------|------|-------------|
| Pfizer       | Drug A   | 100  | 80          |
| Moderna      | Drug B   | 200  | 150         |
| Pfizer       | Drug C   | 300  | 400         |

**Output:**

| manufacturer | drug_count | total_loss |
|--------------|------------|------------|
| Pfizer       | 1          | 20         |
| Moderna      | 1          | 50         |

### Explanation:

- Pfizer has one non-profitable drug (Drug A), with a total loss of $20.
- Moderna has one non-profitable drug (Drug B), with a total loss of $50.

---

## Solution

```sql
WITH cte1 AS (
    SELECT manufacturer, drug, cogs - total_sales AS total_loss
    FROM pharmacy_sales
    WHERE total_sales - cogs < 0
)

SELECT manufacturer, COUNT(drug) AS drug_count, SUM(total_loss) AS total_loss
FROM cte1
GROUP BY manufacturer
ORDER BY total_loss DESC;
```

## Solution Explanation

- The common table expression (CTE) `cte1` calculates the total loss for each non-profitable drug by subtracting `total_sales` from `cogs` and filters for drugs where `total_sales - cogs < 0`.
- The outer query groups the result by `manufacturer`, counts the number of non-profitable drugs, and sums the total loss for each manufacturer.
- The result is ordered by `total_loss` in descending order.
