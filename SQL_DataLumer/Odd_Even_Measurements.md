
# Odd Even Measurements

## Question

You are given a table `measurements` that contains `measurement_id`, `measurement_value`, and `measurement_time`. Write an SQL query to calculate the sum of measurement values for odd-numbered measurements and even-numbered measurements on each day, based on the order of their measurement time.

Return the result with `date`, `odd_sum`, and `even_sum` of measurement values, ordered by `date`.

### Example:

**Measurements Table:**

| measurement_id | measurement_value | measurement_time     |
|----------------|-------------------|----------------------|
| 1              | 10                | 2023-01-01 08:00:00  |
| 2              | 20                | 2023-01-01 09:00:00  |
| 3              | 30                | 2023-01-02 08:00:00  |

**Output:**

| date       | odd_sum | even_sum |
|------------|---------|----------|
| 2023-01-01 | 10      | 20       |
| 2023-01-02 | 30      | NULL     |

---

## Solution

```sql
WITH cte1 AS (
    SELECT DATE(measurement_time) AS date, 
           measurement_id, 
           measurement_value, 
           measurement_time, 
           ROW_NUMBER() OVER (PARTITION BY EXTRACT(DAY FROM measurement_time) ORDER BY measurement_time) AS rnk
    FROM measurements
)

SELECT date, 
       SUM(CASE WHEN rnk % 2 != 0 THEN measurement_value END) AS odd_sum, 
       SUM(CASE WHEN rnk % 2 = 0 THEN measurement_value END) AS even_sum
FROM cte1
GROUP BY date
ORDER BY date;
```

## Solution Explanation

- The common table expression (CTE) `cte1` calculates the row number (`rnk`) for each measurement within each day, ordered by `measurement_time`.
- The main query sums the `measurement_value` for odd-numbered (`rnk % 2 != 0`) and even-numbered (`rnk % 2 = 0`) measurements on each day and groups the result by `date`.
- The result is ordered by `date`.
