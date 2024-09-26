
# Friend Requests II: Who Has the Most Friends

## Question

Write an SQL query to find the person who has the most friends. A person can become friends by either accepting or requesting a friend.

Return the result table ordered by the number of friends in descending order, and return the top result.

### Example:

**RequestAccepted Table:**

| requester_id | accepter_id |
|--------------|-------------|
| 1            | 2           |
| 2            | 3           |
| 3            | 4           |
| 1            | 3           |

**Output:**

| id | num |
|----|-----|
| 1  | 3   |

### Explanation:

- Person 1 has 3 friends: person 2, person 3, and person 4.
- Person 2 has 2 friends.
- Person 3 has 2 friends.
- Person 4 has 1 friend.

The person with the most friends is person 1.

---

## Solution

```sql
WITH cte1 AS (
    SELECT requester_id AS id, COUNT(accepter_id) AS num
    FROM RequestAccepted
    GROUP BY requester_id
),
cte2 AS (
    SELECT accepter_id AS id, COUNT(requester_id) AS num
    FROM RequestAccepted
    GROUP BY accepter_id
)
SELECT id, SUM(num) AS num
FROM (
    (SELECT * FROM cte1)
    UNION ALL
    (SELECT * FROM cte2)
) x
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```

## Solution Explanation

- The first CTE (`cte1`) calculates the number of friends made by requesting friendships (requester_id).
- The second CTE (`cte2`) calculates the number of friends made by accepting friendships (accepter_id).
- The `UNION ALL` combines the results, and the outer query sums the number of friends for each person (`id`).
- The result is ordered by the total number of friends (`num`), and the top result is returned using `LIMIT 1`.
