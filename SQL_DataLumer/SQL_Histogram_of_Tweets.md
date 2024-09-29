
# SQL Histogram of Tweets

## Question

You are given a table `tweets` that contains `user_id`, `tweet_id`, and `tweet_date`. Write an SQL query to create a histogram showing how many users posted a specific number of tweets in the year 2022.

Return the result with the columns `tweet_bucket` (the number of tweets posted by users) and `users_num` (the number of users who posted that many tweets).

### Example:

**Tweets Table:**

| user_id | tweet_id | tweet_date |
|---------|----------|------------|
| 1       | 1001     | 2022-05-01 |
| 1       | 1002     | 2022-05-03 |
| 2       | 1003     | 2022-06-15 |
| 2       | 1004     | 2022-06-20 |
| 2       | 1005     | 2022-07-01 |

**Output:**

| tweet_bucket | users_num |
|--------------|-----------|
| 2            | 1         |
| 3            | 1         |

### Explanation:

- User 1 posted 2 tweets in 2022, and user 2 posted 3 tweets in 2022.
- The output shows the number of users who posted 2 tweets and the number of users who posted 3 tweets.

---

## Solution

```sql
SELECT cnt AS tweet_bucket, COUNT(user_id) AS users_num
FROM (
    SELECT user_id, COUNT(DISTINCT tweet_id) AS cnt
    FROM tweets
    WHERE EXTRACT(YEAR FROM tweet_date) = 2022
    GROUP BY user_id
) x
GROUP BY cnt 
ORDER BY tweet_bucket;
```

## Solution Explanation

- The inner query calculates the number of distinct tweets (`cnt`) posted by each user in 2022 by grouping the data by `user_id`.
- The outer query groups the result by the number of tweets (`cnt`) and counts how many users fall into each tweet bucket.
- The result is ordered by `tweet_bucket` to show the number of users for each number of tweets posted in 2022.
