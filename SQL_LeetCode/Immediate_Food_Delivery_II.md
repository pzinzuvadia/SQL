
# Immediate Food Delivery II

## Question

Write an SQL query to find the **immediate delivery percentage** for the first order of each customer.

The **immediate delivery percentage** is defined as the percentage of customers whose first order was delivered on their preferred delivery date.

Return the result table in **any order**.

### Example:

**Delivery Table:**

| delivery_id | customer_id | order_date | customer_pref_delivery_date |
|-------------|-------------|------------|-----------------------------|
| 1           | 1           | 2021-01-01 | 2021-01-01                  |
| 2           | 1           | 2021-01-02 | 2021-01-02                  |
| 3           | 2           | 2021-01-01 | 2021-01-02                  |
| 4           | 2           | 2021-01-02 | 2021-01-02                  |
| 5           | 3           | 2021-01-01 | 2021-01-01                  |

**Output:**

| immediate_percentage |
|----------------------|
| 66.67                |

### Explanation:

- Customer 1 had their first order on 2021-01-01, which matches their preferred delivery date, so this is an immediate delivery.
- Customer 2 had their first order on 2021-01-01, but their preferred delivery date was 2021-01-02, so this is not an immediate delivery.
- Customer 3 had their first order on 2021-01-01, which matches their preferred delivery date, so this is an immediate delivery.

Out of 3 customers, 2 received immediate deliveries, so the immediate delivery percentage is 2/3 = 66.67.

---

## Solution

```sql
WITH cte1 AS (
    SELECT customer_id, order_date, customer_pref_delivery_date, rn
    FROM (
        SELECT delivery_id, customer_id, order_date, customer_pref_delivery_date, ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date) AS rn
        FROM delivery
    ) x
    WHERE rn = 1
)

SELECT ROUND((COUNT(CASE WHEN order_date = customer_pref_delivery_date THEN 1 END) / COUNT(customer_id)) * 100, 2) AS immediate_percentage
FROM cte1;
```

## Solution Explanation

- The first common table expression (CTE), `cte1`, calculates the **first order** for each customer using `ROW_NUMBER` and filters out only the first order for each customer.
- The final query calculates the percentage of customers whose first order was delivered on their preferred delivery date by comparing `order_date` and `customer_pref_delivery_date`.
- The `ROUND` function is used to round the percentage to two decimal places.
