
# Alibaba Compressed Mode

## Question

You are given a table `items_per_order` that contains `item_count` (the number of items in an order) and `order_occurrences` (the number of times an order with that item count occurred). Write an SQL query to find the mode of the item count, which is the item count that occurs most frequently.

Return the result with `item_count` as `mode`, ordered by `item_count`.

### Example:

**Items Per Order Table:**

| item_count | order_occurrences |
|------------|-------------------|
| 1          | 100               |
| 2          | 150               |
| 3          | 150               |

**Output:**

| mode |
|------|
| 2    |
| 3    |

---

## Solution

```sql
WITH cte1 AS (
    SELECT item_count, order_occurrences, 
           DENSE_RANK() OVER (ORDER BY order_occurrences DESC) AS rnk 
    FROM items_per_order
)

SELECT item_count AS mode
FROM cte1
WHERE rnk = 1
ORDER BY item_count;
```

## Solution Explanation

- The query uses a common table expression (CTE) `cte1` to rank the item counts based on the number of order occurrences using the `DENSE_RANK()` function.
- The main query filters the result to include only the item counts with the highest number of occurrences (`rnk = 1`) and orders the result by `item_count`.
