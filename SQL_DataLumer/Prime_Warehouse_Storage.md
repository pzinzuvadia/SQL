
# Prime Warehouse Storage

## Question

You are given a table `inventory` that contains `item_id`, `item_type` (either 'prime_eligible' or 'not_prime'), and `square_footage` (the amount of space occupied by the item in the warehouse). The warehouse has a total of 500,000 square feet of storage space, and your task is to determine how many items of each type can fit in the warehouse.

Write an SQL query to calculate the total number of 'prime_eligible' and 'not_prime' items that can fit in the warehouse, assuming that 'prime_eligible' items are given storage priority.

Return the result with `item_type` and `item_count`, ordered by `item_type` in descending order.

### Example:

**Inventory Table:**

| item_id | item_type      | square_footage |
|---------|----------------|----------------|
| 1       | prime_eligible | 1000           |
| 2       | not_prime      | 1500           |

**Output:**

| item_type      | item_count |
|----------------|------------|
| prime_eligible | 500        |
| not_prime      | 300        |

---

## Solution

```sql
WITH cte1 AS (
    SELECT item_type, SUM(square_footage) AS sq_ft_per_batch, 
           COUNT(item_id) AS item_per_batch
    FROM inventory
    GROUP BY item_type
),

cte2 AS (
    SELECT item_type, prime_batches, prime_batches * item_per_batch AS tot_prime_cnt, sq_ft_per_batch
    FROM (
        SELECT item_type, FLOOR(500000 / sq_ft_per_batch) AS prime_batches, item_per_batch, sq_ft_per_batch
        FROM cte1
        WHERE item_type = 'prime_eligible'
    ) x
),

cte3 AS (
    SELECT item_type, non_prime_batches, non_prime_batches * item_per_batch AS tot_non_prime_cnt, sq_ft_per_batch
    FROM (
        SELECT item_type, FLOOR((SELECT 500000 - (prime_batches * sq_ft_per_batch) FROM cte2) / sq_ft_per_batch) AS non_prime_batches, item_per_batch, sq_ft_per_batch
        FROM cte1
        WHERE item_type = 'not_prime'
    ) y
)

SELECT * FROM (
    SELECT item_type, tot_non_prime_cnt AS item_count
    FROM cte3
    UNION  
    SELECT item_type, tot_prime_cnt AS item_count
    FROM cte2
) a  
ORDER BY item_type DESC;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` calculates the total square footage per batch and the number of items per batch for each item type.
- The second CTE `cte2` calculates how many 'prime_eligible' batches can fit into the warehouse based on the total available square footage.
- The third CTE `cte3` calculates how many 'not_prime' batches can fit into the remaining space after 'prime_eligible' items have been accounted for.
- The result returns the total item count for both 'prime_eligible' and 'not_prime' items, ordered by `item_type`.
