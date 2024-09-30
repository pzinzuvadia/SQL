
# SQL Average Post Hiatus 1

## Question

You are given a table `posts` that contains `user_id` and `post_date`. Write an SQL query to find the number of days between the first and last posts made by each user in the year 2021, but only for users who made more than one post during that year.

Return the result with the `user_id` and the number of `days_between` their first and last posts in 2021.

### Example:

**Posts Table:**

| user_id | post_date |
|---------|-----------|
| 1       | 2021-01-01 |
| 1       | 2021-03-05 |
| 2       | 2021-02-15 |
| 2       | 2021-07-20 |
| 3       | 2021-12-25 |

**Output:**

| user_id | days_between |
|---------|--------------|
| 1       | 63           |
| 2       | 155          |

### Explanation:

- User 1 posted on January 1st and again on March 5th, so the number of days between their first and last posts is 63.
- User 2 posted on February 15th and again on July 20th, so the number of days between their first and last posts is 155.
- User 3 made only one post in 2021, so they are excluded from the result.

---

## Solution

```sql
WITH cte1 AS (
    SELECT user_id, MIN(post_date) AS first_post, MAX(post_date) AS last_post
    FROM posts
    WHERE EXTRACT(YEAR FROM post_date) = 2021
    GROUP BY user_id
    HAVING MIN(post_date) != MAX(post_date)
)

SELECT user_id, EXTRACT(DAY FROM (last_post - first_post)) AS days_between
FROM cte1;
```

## Solution Explanation

- The common table expression (CTE) `cte1` identifies users who made multiple posts in 2021 by selecting the minimum (`first_post`) and maximum (`last_post`) post dates for each `user_id` and filtering out users who only made one post using the `HAVING` clause.
- The main query calculates the number of days between the first and last posts for each user using the `EXTRACT(DAY FROM ...)` function.
