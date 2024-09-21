
# Biggest Single Number

## Question

Write an SQL query to find the largest number that appears only once in the table.

If no such number exists, return null.

Return the result table with a single column named `num`.

### Example:

**Mynumbers Table:**

| num |
|-----|
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 4   |

**Output:**

| num |
|-----|
| 1   |

### Explanation:

- The number 1 appears exactly once, and it is the largest among the numbers that appear only once.

---

## Solution

```sql
SELECT CASE WHEN COUNT(num) != 0 THEN num ELSE NULL END AS num
FROM (
    SELECT num
    FROM mynumbers
    GROUP BY num
    HAVING COUNT(*) = 1
    ORDER BY num DESC
    LIMIT 1
) x;
```

## Solution Explanation

- The inner query selects numbers that appear only once using the `HAVING COUNT(*) = 1` condition.
- It then orders the result by `num` in descending order and limits it to the largest single number using `LIMIT 1`.
- The outer query checks if there is any result. If so, it returns the number; otherwise, it returns `NULL`.
