
# User Activity for the Past 30 Days I

## Question

Write an SQL query to find the number of **active users** for each day within the past 30 days from '2019-07-27'.

An active user is defined as a user who has performed any activity (i.e., the `activity_type` is not null) on that day.

Return the result table in **any order**.

### Example:

**Activity Table:**

| user_id | activity_date | activity_type |
|---------|----------------|---------------|
| 1       | 2019-07-27     | Login         |
| 1       | 2019-07-27     | Logout        |
| 2       | 2019-07-26     | Login         |
| 3       | 2019-07-26     | Login         |
| 4       | 2019-07-25     | Login         |
| 5       | 2019-07-24     | Logout        |
| 6       | 2019-07-23     | Login         |

**Output:**

| day        | active_users |
|------------|--------------|
| 2019-07-27 | 1            |
| 2019-07-26 | 2            |
| 2019-07-25 | 1            |
| 2019-07-24 | 1            |
| 2019-07-23 | 1            |

### Explanation:

- On 2019-07-27, 1 user logged in or out, so the active users count is 1.
- On 2019-07-26, 2 users performed activities, so the active users count is 2.

---

## Solution

```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM activity
WHERE (activity_date BETWEEN DATE_ADD('2019-07-27', INTERVAL -29 DAY) AND '2019-07-27') 
  AND (activity_type IS NOT NULL)
GROUP BY activity_date;
```

## Solution Explanation

- The `WHERE` clause filters the records to only include activities within the past 30 days, using `DATE_ADD` to calculate the start date and ensuring that the `activity_type` is not null.
- The `COUNT(DISTINCT user_id)` function counts the number of unique active users for each day.
- The `GROUP BY` clause groups the result by `activity_date` to generate the daily active user count.
