
# Duplicate Job Listings

## Question

You are given a table `job_listings` that contains `company_id`, `job_id`, `title`, and `description`. Write an SQL query to find how many companies have posted duplicate job listings, where duplicate job listings are defined as having the same `title` and `description` within the same company.

Return the result as `duplicate_companies`, which is the number of companies with duplicate job listings.

### Example:

**Job Listings Table:**

| company_id | job_id | title       | description |
|------------|--------|-------------|-------------|
| 1          | 101    | Engineer    | Job Desc 1  |
| 1          | 102    | Engineer    | Job Desc 1  |
| 2          | 103    | Developer   | Job Desc 2  |
| 2          | 104    | Developer   | Job Desc 3  |

**Output:**

| duplicate_companies |
|---------------------|
| 1                   |

### Explanation:

- Company 1 posted the same job twice (same `title` and `description`), so it is counted as a duplicate.
- Company 2 posted two different job listings, so it is not counted.

---

## Solution

```sql
SELECT COUNT(company_id) AS duplicate_companies
FROM (
    SELECT company_id, title, description, COUNT(DISTINCT job_id) AS job_cnt
    FROM job_listings
    GROUP BY company_id, title, description
) x
WHERE job_cnt > 1;
```

## Solution Explanation

- The inner query groups the `job_listings` by `company_id`, `title`, and `description`, and counts the distinct `job_id` for each group.
- The outer query filters the results to include only those groups where more than one job listing (`job_cnt > 1`) exists, indicating duplicate job listings for that company.
- The final result counts the number of companies that have duplicate job listings.
