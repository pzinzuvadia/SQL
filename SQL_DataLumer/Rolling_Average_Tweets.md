
# Rolling Average Tweets

## Question

You are given a table `tweets` that contains `user_id`, `tweet_date`, and `tweet_count` (the number of tweets posted on that date). Write an SQL query to calculate the 3-day rolling average of the number of tweets for each user, where the rolling window includes the current day and the two preceding days.

Return the result with `user_id`, `tweet_date`, and the `rolling_avg_3d` (the 3-day rolling average of tweets) rounded to two decimal places.

### Example:

**Tweets Table:**

| user_id | tweet_date | tweet_count |
|---------|------------|-------------|
| 1       | 2023-07-01 | 5           |
| 1       | 2023-07-02 | 10          |
| 1       | 2023-07-03 | 15          |
| 2       | 2023-07-01 | 8           |
| 2       | 2023-07-02 | 12          |

**Output:**

| user_id | tweet_date | rolling_avg_3d |
|---------|------------|----------------|
| 1       | 2023-07-01 | 5.00           |
| 1       | 2023-07-02 | 7.50           |
| 1       | 2023-07-03 | 10.00          |
| 2       | 2023-07-01 | 8.00           |
| 2       | 2023-07-02 | 10.00          |

### Explanation:

- For user 1 on July 1st, the rolling average is 5.00 (only the first day's tweets).
- For user 1 on July 2nd, the rolling average is (5 + 10) / 2 = 7.50.
- For user 1 on July 3rd, the rolling average is (5 + 10 + 15) / 3 = 10.00.
- For user 2 on July 1st, the rolling average is 8.00.
- For user 2 on July 2nd, the rolling average is (8 + 12) / 2 = 10.00.

---

## Solution

```sql
SELECT user_id, tweet_date, 
       ROUND(AVG(tweet_count) OVER (PARTITION BY user_id ORDER BY tweet_date 
                                    RANGE BETWEEN INTERVAL '2 days' PRECEDING AND CURRENT ROW), 2) AS rolling_avg_3d
FROM tweets;
```

## Solution Explanation

- The query calculates the 3-day rolling average of `tweet_count` for each `user_id` using the `AVG()` window function.
- The `PARTITION BY user_id` ensures the rolling average is calculated separately for each user.
- The `ORDER BY tweet_date` arranges the rows in date order for each user, and the `RANGE BETWEEN INTERVAL '2 days' PRECEDING AND CURRENT ROW` defines the 3-day window for the rolling average.
- The result is rounded to two decimal places using the `ROUND()` function.
