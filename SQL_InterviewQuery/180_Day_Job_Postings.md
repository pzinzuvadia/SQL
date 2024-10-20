
# 180-Day Job Postings

## Question

You are given a table `job_postings` that contains `job_posting_id`, `job_id`, `user_id`, `date_posted`, and `is_revoked`. Write an SQL query to calculate the percentage of job postings that were revoked within 180 days of being posted.

Return the result as the percentage, rounded to two decimal places.

### Example:

**Job Postings Table:**

| job_posting_id | job_id | user_id | date_posted | is_revoked |
|----------------|--------|---------|-------------|------------|
| 1              | 1001   | 200     | 2023-01-01  | 0          |
| 2              | 1002   | 201     | 2023-06-01  | 1          |
| 3              | 1003   | 202     | 2023-03-15  | 1          |

**Output:**

| percentage |
|------------|
| 66.67      |

---

## Solution

```sql
WITH cte1 AS (
    SELECT job_id, date_posted, most_recent, is_revoked
    FROM (
        SELECT job_posting_id, job_id, user_id, date_posted, is_revoked, 
               MAX(date_posted) OVER() AS most_recent, 
               ROW_NUMBER() OVER (PARTITION BY job_id ORDER BY job_posting_id DESC) AS rnk
        FROM job_postings jp
    ) x
    WHERE rnk = 1
)

SELECT ROUND(
           (COUNT(CASE WHEN is_revoked = 1 AND DATEDIFF(most_recent, date_posted) <= 180 THEN 1 END) 
            / COUNT(job_id)) * 100, 2
       ) AS percentage
FROM cte1;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` selects the most recent job posting for each job by using the `ROW_NUMBER()` function to partition the table by `job_id` and order the rows by `job_posting_id` in descending order.
- It also calculates the most recent `date_posted` using the `MAX()` function.
- The main query calculates the percentage of job postings that were revoked within 180 days of being posted by checking if `is_revoked` is 1 and the difference between `most_recent` and `date_posted` is less than or equal to 180 days.
- The result is rounded to two decimal places and returned as the percentage.
