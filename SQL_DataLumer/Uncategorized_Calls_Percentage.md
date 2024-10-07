
# Uncategorized Calls Percentage

## Question

You are given a table `callers` that contains information about calls, including the column `call_category`. Write an SQL query to calculate the percentage of calls that are uncategorized, where an uncategorized call is defined as having a `NULL` or `'n/a'` value in the `call_category` column.

Return the result as `uncategorized_call_pct`, which represents the percentage of uncategorized calls, rounded to one decimal place.

### Example:

**Callers Table:**

| caller_id | call_category |
|-----------|---------------|
| 1         | NULL          |
| 2         | personal      |
| 3         | n/a           |

**Output:**

| uncategorized_call_pct |
|------------------------|
| 66.7                   |

### Explanation:

- 66.7% of the calls are uncategorized, as 2 out of 3 calls have a `NULL` or `'n/a'` category.

---

## Solution

```sql
SELECT ROUND(100.0 * COUNT(CASE WHEN call_category IS NULL OR call_category = 'n/a' THEN 1 END) / COUNT(*), 1) AS uncategorized_call_pct
FROM callers;
```

## Solution Explanation

- The query counts the number of calls where the `call_category` is `NULL` or `'n/a'` and divides it by the total number of calls.
- The result is multiplied by 100 to get the percentage of uncategorized calls and is rounded to one decimal place using the `ROUND()` function.
