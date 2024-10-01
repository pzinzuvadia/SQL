
# Alibaba Compressed Mean

## Question

You are given a table `items_per_order` that contains `item_count` (the number of items in an order) and `order_occurrences` (the number of times an order with that item count occurred). Write an SQL query to calculate the weighted mean of the item count, where the weight is the number of order occurrences.

The weighted mean is calculated as:

\[
	ext{mean} = rac{\sum (	ext{item_count} 	imes 	ext{order_occurrences})}{\sum 	ext{order_occurrences}}
\]

Return the result as `mean` rounded to one decimal place.

### Example:

**Items Per Order Table:**

| item_count | order_occurrences |
|------------|-------------------|
| 1          | 100               |
| 2          | 50                |
| 3          | 25                |

**Output:**

| mean |
|------|
| 1.5  |

### Explanation:

- The weighted mean is calculated as:

\[
rac{(1 	imes 100) + (2 	imes 50) + (3 	imes 25)}{100 + 50 + 25} = 1.5
\]

---

## Solution

```sql
SELECT ROUND(CAST(SUM(item_count * order_occurrences) / SUM(order_occurrences) AS NUMERIC), 1) AS mean
FROM items_per_order;
```

## Solution Explanation

- The query calculates the weighted mean of the `item_count`, using `order_occurrences` as the weight.
- The `SUM(item_count * order_occurrences)` calculates the total weighted item count, and `SUM(order_occurrences)` calculates the total occurrences.
- The result is rounded to one decimal place using the `ROUND` function.
