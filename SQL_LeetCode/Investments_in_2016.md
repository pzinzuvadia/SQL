
# Investments in 2016

## Question

Write an SQL query to find the total investment value (TIV) in 2016. The result should include only distinct investment values that meet the following conditions:
1. The same TIV was present in 2015.
2. The location (latitude and longitude) of the investment did not appear more than once.

Return the result as `tiv_2016` rounded to two decimal places.

### Example:

**Insurance Table:**

| tiv_2015 | tiv_2016 | lat   | lon   |
|----------|----------|-------|-------|
| 10.0     | 12.0     | 45.0  | 60.0  |
| 10.0     | 15.0     | 45.0  | 60.0  |
| 20.0     | 25.0     | 50.0  | 65.0  |

**Output:**

| tiv_2016 |
|----------|
| 25.00    |

### Explanation:

- The TIV of 10.0 in 2015 is present more than once, so it is excluded.
- The location (45.0, 60.0) appears more than once, so those records are excluded.
- Only the record with `tiv_2016` of 25.0 satisfies both conditions.

---

## Solution

```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015 
    FROM Insurance 
    GROUP BY tiv_2015 
    HAVING COUNT(*) > 1
)
AND (lat, lon) NOT IN (
    SELECT lat, lon 
    FROM Insurance 
    GROUP BY lat, lon 
    HAVING COUNT(*) > 1
);
```

## Solution Explanation

- The subquery in the `WHERE` clause selects TIVs from 2015 that appear more than once using the `GROUP BY` and `HAVING` clauses.
- The second subquery filters out locations (latitude and longitude) that appear more than once.
- The `SUM` function calculates the total TIV in 2016 for the remaining records, and the `ROUND` function rounds the result to two decimal places.
