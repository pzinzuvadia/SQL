
# SQL Swapped Food Delivery

## Question

You are given a table `orders` that contains `order_id` and `item`. Due to a system error, some food orders were swapped between adjacent orders. Write an SQL query to correct the swapped orders by swapping the items back to their correct orders.

Return the result with `corrected_order_id` and `item`, where the item is swapped back to its correct order. If there is no swap for an order, return the original item.

### Example:

**Orders Table:**

| order_id | item      |
|----------|-----------|
| 1        | Burger    |
| 2        | Pizza     |
| 3        | Salad     |

**Output:**

| corrected_order_id | item      |
|--------------------|-----------|
| 1                  | Pizza     |
| 2                  | Burger    |
| 3                  | Salad     |

---

## Solution

```sql
WITH cte1 AS (
    SELECT order_id, item, 
           LEAD(item, 1) OVER (ORDER BY order_id) AS next_item, 
           LAG(item) OVER (ORDER BY order_id) AS prv_item
    FROM orders
)

SELECT order_id AS corrected_order_id, 
       COALESCE(CASE WHEN order_id % 2 != 0 THEN next_item ELSE prv_item END, item) AS item
FROM cte1;
```

## Solution Explanation

- The query uses a common table expression (CTE) `cte1` to identify the `next_item` and `previous_item` for each order using the `LEAD()` and `LAG()` window functions.
- The main query swaps the items back to their correct orders by using the `CASE` statement to swap items for odd-numbered orders with the next item and for even-numbered orders with the previous item.
- The `COALESCE()` function ensures that if there is no swap (e.g., the first or last order), the original item is returned.
