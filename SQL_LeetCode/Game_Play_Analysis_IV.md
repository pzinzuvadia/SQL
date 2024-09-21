
# Game Play Analysis IV

## Question

Write an SQL query to find the **fraction** of players who logged in again on the day after their first login.

The result should be rounded to **two decimal places**.

### Example:

**Activity Table:**

| player_id | event_date |
|-----------|------------|
| 1         | 2020-03-01 |
| 1         | 2020-03-02 |
| 2         | 2020-03-01 |
| 2         | 2020-03-04 |
| 3         | 2020-03-01 |
| 3         | 2020-03-02 |
| 3         | 2020-03-03 |

**Output:**

| fraction |
|----------|
| 0.33     |

### Explanation:

- Player 1 logged in again on 2020-03-02, the day after their first login.
- Player 2 did not log in the day after their first login.
- Player 3 logged in again on 2020-03-02, the day after their first login.

There are 3 players in total, and 1 out of 3 players logged in the day after their first login, so the fraction is 1/3 = 0.33.

---

## Solution

```sql
WITH cte1 (player_id, first_login) AS (
    SELECT player_id, event_date
    FROM (
        SELECT player_id, event_date, ROW_NUMBER() OVER(PARTITION BY player_id ORDER BY event_date) AS first_login
        FROM activity
    ) x
    WHERE first_login = 1
),

cte2 (player_id, next_login) AS (
    SELECT player_id, next_login
    FROM (
        SELECT player_id, event_date, LEAD(event_date) OVER(PARTITION BY player_id ORDER BY event_date) AS next_login
        FROM activity
    ) y
)

SELECT ROUND(COUNT(c1.player_id) / (SELECT COUNT(DISTINCT player_id) FROM activity), 2) AS fraction
FROM cte1 c1
JOIN cte2 c2
ON c1.player_id = c2.player_id
WHERE DATEDIFF(c2.next_login, c1.first_login) = 1;
```

## Solution Explanation

- The first common table expression (CTE), `cte1`, calculates the **first login** for each player using `ROW_NUMBER` and filters out only the first login per player.
- The second CTE, `cte2`, calculates the **next login** after the first login using the `LEAD` function.
- The final query joins `cte1` and `cte2` on `player_id` and checks whether the difference between the first and next login dates is 1 day.
- The `COUNT` function calculates the number of players who logged in the day after their first login, and this is divided by the total number of distinct players to calculate the fraction.
- The result is rounded to two decimal places using `ROUND`.
