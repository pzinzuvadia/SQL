
# Median Search Frequency

## Question

You are given a table `search_frequency` that contains `searches` (the number of searches a user performed) and `num_users` (the number of users who performed that many searches). Write an SQL query to find the median number of searches performed by users.

Return the result as the median number of searches, rounded to one decimal place.

### Example:

**Search Frequency Table:**

| searches | num_users |
|----------|-----------|
| 1        | 3         |
| 2        | 5         |
| 3        | 2         |

**Output:**

| median |
|--------|
| 2.0    |

---

## Solution

```sql
WITH cte1 AS (
    SELECT searches, LEAD(searches) OVER (ORDER BY searches) AS next, 
           series, ROW_NUMBER() OVER (ORDER BY searches, series) AS rn, 
           tot_users
    FROM (
        SELECT searches, GENERATE_SERIES(1, num_users) AS series, 
               SUM(num_users) OVER () AS tot_users
        FROM search_frequency
    ) x
)

SELECT ROUND(median, 1)
FROM (
    SELECT CASE 
               WHEN tot_users % 2 = 0 AND rn = tot_users / 2 THEN (searches + next) / 2.0
               WHEN tot_users % 2 = 1 AND rn = CEIL(tot_users / 2) THEN searches
           END AS median
    FROM cte1
) y  
WHERE median IS NOT NULL;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` expands the `search_frequency` table by generating a series of rows for each user, with each row corresponding to a single user performing a certain number of searches.
- The main query then calculates the median number of searches by checking whether the total number of users is even or odd and selecting the appropriate median value.
- The result is rounded to one decimal place and returned as the median search frequency.
