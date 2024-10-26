
# 180-Day Job Postings (Revoked)

## Question

You are given a table `job_postings` that contains `job_posting_id`, `job_id`, `date_posted`, and `is_revoked`. Write an SQL query to calculate the percentage of job postings that were revoked within the past 180 days from the latest posting date.

Return the result as the percentage, rounded to two decimal places.

### Example:

**Job Postings Table:**

| job_posting_id | job_id | date_posted | is_revoked |
|----------------|--------|-------------|------------|
| 1              | 1001   | 2023-01-01  | 0          |
| 2              | 1002   | 2023-06-01  | 1          |
| 3              | 1003   | 2023-03-15  | 1          |

**Output:**

| percentage |
|------------|
| 66.67      |

---

## Solution

```sql
WITH cte1 AS (
    SELECT job_posting_id, job_id, is_revoked
    FROM (
        SELECT job_posting_id, job_id, is_revoked, date_posted, 
               DENSE_RANK() OVER (PARTITION BY job_id ORDER BY date_posted DESC) AS rnk
        FROM job_postings
        WHERE DATEDIFF((SELECT MAX(date_posted) FROM job_postings), date_posted) <= 180
    ) x
    WHERE rnk = 1
)

SELECT ROUND((COUNT(CASE WHEN is_revoked = 1 THEN 1 END) / COUNT(*)) * 100, 2) AS percentage
FROM cte1;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` selects the most recent job posting for each job within the past 180 days by using the `DENSE_RANK()` function to partition by `job_id` and order by `date_posted` in descending order.
- The main query calculates the percentage of job postings that were revoked within this 180-day window by counting the `is_revoked` entries and dividing by the total count.
- The result is rounded to two decimal places and returned as the percentage.
