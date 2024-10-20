
# Always Excited Users

## Question

You are given a table `ad_impressions` that contains `user_id`, `impression_id`, and `dt` (the date of the impression). Each `impression_id` can either be 'Excited' or 'Bored'. Write an SQL query to find all users whose most recent impression was 'Excited' and who never had a 'Bored' impression.

Return the result as a list of `user_id`s.

### Example:

**Ad Impressions Table:**

| user_id | impression_id | dt       |
|---------|---------------|----------|
| 1       | Excited       | 2023-01-01 |
| 2       | Bored         | 2023-01-02 |
| 1       | Excited       | 2023-01-03 |
| 3       | Excited       | 2023-01-04 |

**Output:**

| user_id |
|---------|
| 1       |
| 3       |

---

## Solution

```sql
SELECT user_id
FROM (
    SELECT user_id, impression_id, ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY dt DESC) AS rnk
    FROM ad_impressions
) x
WHERE rnk = 1 AND impression_id = 'Excited' 
  AND user_id IN (
      SELECT user_id
      FROM ad_impressions
      GROUP BY user_id
      HAVING COUNT(CASE WHEN impression_id = 'Bored' THEN 1 END) = 0
  );
```

## Solution Explanation

- The inner query assigns a row number to each impression for each user, ordering by the date (`dt`) in descending order. This allows us to identify the most recent impression for each user.
- The outer query filters for users whose most recent impression is 'Excited' and who have never had a 'Bored' impression.
- The final result returns the list of `user_id`s that meet both conditions.
