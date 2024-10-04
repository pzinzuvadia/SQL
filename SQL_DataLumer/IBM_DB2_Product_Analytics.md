
# IBM DB2 Product Analytics

## Question

You are given two tables: `queries` and `employees`. The `queries` table contains `employee_id`, `query_id`, and `query_starttime`, and the `employees` table contains `employee_id`. Write an SQL query to find how many employees executed a specific number of distinct queries between July 1, 2023, and September 30, 2023.

Return the result with `unique_queries` (the number of distinct queries executed) and `employee_count` (the number of employees who executed that many distinct queries), ordered by `unique_queries` in ascending order.

### Example:

**Queries Table:**

| employee_id | query_id | query_starttime       |
|-------------|----------|-----------------------|
| 1           | 101      | 2023-07-01 10:00:00   |
| 1           | 102      | 2023-08-01 12:00:00   |
| 2           | 103      | 2023-07-01 09:00:00   |

**Employees Table:**

| employee_id |
|-------------|
| 1           |
| 2           |
| 3           |

**Output:**

| unique_queries | employee_count |
|----------------|----------------|
| 0              | 1              |
| 2              | 1              |
| 1              | 1              |

### Explanation:

- Employee 1 executed 2 distinct queries.
- Employee 2 executed 1 distinct query.
- Employee 3 did not execute any queries, so their query count is 0.

---

## Solution

```sql
WITH cte1 AS (
    SELECT employee_id, COUNT(DISTINCT query_id) AS queries
    FROM queries
    WHERE query_starttime >= '2023-07-01 00:00:00' 
      AND query_starttime < '2023-10-01 00:00:00'
    GROUP BY employee_id
),

cte2 AS (
    SELECT DISTINCT e.employee_id, 
           CASE WHEN c.queries IS NULL THEN 0 ELSE c.queries END AS cnt
    FROM employees e  
    LEFT JOIN cte1 c  
    ON c.employee_id = e.employee_id
)

SELECT cnt AS unique_queries, 
       COUNT(DISTINCT employee_id) AS employee_count
FROM cte2
GROUP BY cnt
ORDER BY unique_queries;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` calculates the number of distinct queries executed by each employee between July 1, 2023, and September 30, 2023.
- The second CTE `cte2` ensures that all employees are included in the result, even if they did not execute any queries (those employees will have a query count of 0).
- The main query groups the result by the number of distinct queries (`cnt`) and counts how many employees fall into each query count bucket.
