
# Acceptance Rate

## Question

You are given two tables: `friend_requests`, which contains `requester_id` and `requested_id`, and `friend_accepts`, which contains `requester_id` and `acceptor_id`. Write an SQL query to calculate the acceptance rate of friend requests.

The acceptance rate is calculated as the ratio of accepted friend requests to the total number of friend requests.

### Example:

**Friend Requests Table:**

| requester_id | requested_id |
|--------------|--------------|
| 1            | 2            |
| 1            | 3            |
| 2            | 3            |

**Friend Accepts Table:**

| requester_id | acceptor_id |
|--------------|-------------|
| 1            | 2           |
| 2            | 3           |

**Output:**

| acceptance_rate |
|-----------------|
| 0.67            |

---

## Solution

```sql
SELECT COUNT(fa.acceptor_id) / COUNT(*) AS acceptance_rate
FROM friend_requests fr 
LEFT JOIN friend_accepts fa 
ON fa.acceptor_id = fr.requested_id AND fa.requester_id = fr.requester_id;
```

## Solution Explanation

- The query performs a `LEFT JOIN` between the `friend_requests` and `friend_accepts` tables based on matching `requester_id` and `requested_id`.
- The `COUNT(fa.acceptor_id)` counts the accepted friend requests, while `COUNT(*)` counts the total friend requests.
- The result is returned as the `acceptance_rate`, which is the ratio of accepted requests to total requests.
