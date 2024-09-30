
# Teams Power Users

## Question

You are given a table `messages` that contains `sender_id`, `message_id`, and `sent_date`. Write an SQL query to find the top two power users who sent the most distinct messages in August 2022.

Return the result with the `sender_id` and `message_count`, ordered by `message_count` in descending order. If there is a tie, return any two users with the highest message count.

### Example:

**Messages Table:**

| sender_id | message_id | sent_date   |
|-----------|------------|-------------|
| 1         | 101        | 2022-08-01  |
| 2         | 102        | 2022-08-01  |
| 1         | 103        | 2022-08-02  |
| 2         | 104        | 2022-08-03  |
| 3         | 105        | 2022-08-04  |

**Output:**

| sender_id | message_count |
|-----------|---------------|
| 1         | 2             |
| 2         | 2             |

### Explanation:

- User 1 and user 2 both sent two distinct messages in August 2022.
- The output shows the top two users with the highest message count.

---

## Solution

```sql
SELECT sender_id, COUNT(DISTINCT message_id) AS message_count
FROM messages
WHERE EXTRACT(YEAR FROM sent_date) = '2022' AND EXTRACT(MONTH FROM sent_date) = '08'
GROUP BY sender_id
ORDER BY message_count DESC
LIMIT 2;
```

## Solution Explanation

- The query filters the `messages` table for messages sent in August 2022 using the `EXTRACT` function for both the year and month.
- It groups the messages by `sender_id` and counts the distinct `message_id` for each user.
- The result is ordered by the number of messages (`message_count`) in descending order, and only the top two users are returned using `LIMIT 2`.
