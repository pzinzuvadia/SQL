
# Consecutive Numbers

## Question

Write an SQL query to find all numbers that appear at least three times consecutively in the `logs` table.

Return the result table in **any order**.

### Example:

**Logs Table:**

| id | num |
|----|-----|
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |

**Output:**

| ConsecutiveNums |
|----------------|
| 1              |

### Explanation:

- The number 1 appears consecutively three times, so it is included in the result.
- The number 2 does not appear three times consecutively.

---

## Solution

```sql
SELECT DISTINCT num AS ConsecutiveNums
FROM (
    SELECT num, LEAD(num) OVER() AS num1, LEAD(num, 2) OVER() AS num2
    FROM logs
) x
WHERE num = num1 AND num1 = num2;
```

## Solution Explanation

- The inner query uses the `LEAD` window function to look at the next two numbers (`num1` and `num2`) in the sequence for each row.
- The outer query checks whether the current number (`num`) is equal to the next two numbers (`num1` and `num2`). If so, it selects that number.
- The `DISTINCT` keyword ensures that each number is returned only once.
