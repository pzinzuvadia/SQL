
# Delete Duplicate Emails

## Question

Write an SQL query to delete all duplicate emails from the `person` table, keeping only the row with the smallest `id` for each email.

Return the result table ordered by `id`.

### Example:

**Person Table:**

| id | email           |
|----|-----------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |

**Output:**

| id | email           |
|----|-----------------|
| 1  | john@example.com |
| 2  | bob@example.com  |

### Explanation:

- The email "john@example.com" appears twice, so the row with the larger `id` is deleted.

---

## Solution

```sql
WITH cte1 AS (
    SELECT MIN(id) AS id
    FROM person
    GROUP BY email
)

DELETE FROM Person
WHERE id NOT IN (SELECT id FROM cte1);
```

## Solution Explanation

- The common table expression (CTE) `cte1` selects the smallest `id` for each email by grouping the `person` table by `email` and using the `MIN(id)` function.
- The `DELETE` statement removes any rows whose `id` is not in the result of the CTE, effectively deleting the duplicate emails.
