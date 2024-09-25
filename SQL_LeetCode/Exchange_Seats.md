
# Exchange Seats

## Question

Write an SQL query to swap the seats of adjacent students in a `seat` table. If there is an odd number of students, the last student remains in the same seat.

Return the result table ordered by `id`.

### Example:

**Seat Table:**

| id | student |
|----|---------|
| 1  | A       |
| 2  | B       |
| 3  | C       |
| 4  | D       |
| 5  | E       |

**Output:**

| id | student |
|----|---------|
| 1  | B       |
| 2  | A       |
| 3  | D       |
| 4  | C       |
| 5  | E       |

### Explanation:

- Seat 1 is swapped with seat 2, seat 3 is swapped with seat 4, and seat 5 remains in the same seat because there is an odd number of students.

---

## Solution

```sql
SELECT CASE WHEN id % 2 != 0 THEN next_id ELSE pre_id END AS id, student
FROM (
    SELECT student, id, LEAD(id, 1, id) OVER() AS next_id, LAG(id, 1, id) OVER() AS pre_id
    FROM seat
) x
ORDER BY id;
```

## Solution Explanation

- The inner query uses the `LEAD` and `LAG` window functions to get the next (`next_id`) and previous (`pre_id`) student seat IDs for each row.
- The outer query uses a `CASE` statement to swap adjacent seats by returning `next_id` for odd-numbered seats and `pre_id` for even-numbered seats.
- The result is ordered by `id` to maintain the original seat order.
