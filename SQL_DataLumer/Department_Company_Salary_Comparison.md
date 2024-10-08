
# Department vs Company Salary Comparison

## Question

You are given two tables: `employee` and `salary`. The `employee` table contains `employee_id` and `department_id`, and the `salary` table contains `employee_id`, `amount`, and `payment_date`. Write an SQL query to compare the average salary for each department in March with the average salary across the entire company for the same month.

Return the result with `department_id`, `mth` (month in 'MM-YYYY' format), and `comparison`, where:
- `comparison = 'higher'` if the department's average salary is higher than the company average,
- `comparison = 'lower'` if it is lower,
- `comparison = 'same'` if it is the same.

### Example:

**Employee Table:**

| employee_id | department_id |
|-------------|---------------|
| 1           | 101           |
| 2           | 101           |
| 3           | 102           |

**Salary Table:**

| employee_id | amount | payment_date |
|-------------|--------|--------------|
| 1           | 1000   | 2023-03-01   |
| 2           | 1200   | 2023-03-01   |
| 3           | 800    | 2023-03-01   |

**Output:**

| department_id | mth     | comparison |
|---------------|---------|------------|
| 101           | 03-2023 | higher     |
| 102           | 03-2023 | lower      |

---

## Solution

```sql
WITH cte1 AS (
    SELECT e.employee_id, e.department_id, s.amount, 
           CONCAT(TO_CHAR(payment_date, 'MM'), '-', EXTRACT(year FROM payment_date)) AS mth
    FROM employee e  
    JOIN salary s 
    ON e.employee_id = s.employee_id
    WHERE EXTRACT(month FROM s.payment_date) = 3
)

SELECT department_id, mth, 
       CASE WHEN dep_avg_sal < (SELECT AVG(amount) FROM salary WHERE EXTRACT(month FROM payment_date) = 3) 
            THEN 'lower' 
            WHEN dep_avg_sal = (SELECT AVG(amount) FROM salary WHERE EXTRACT(month FROM payment_date) = 3) 
            THEN 'same'
            ELSE 'higher' 
       END AS comparison
FROM (
    SELECT department_id, AVG(amount) AS dep_avg_sal, mth
    FROM cte1
    GROUP BY department_id, mth
) x;
```

## Solution Explanation

- The common table expression (CTE) `cte1` filters the `salary` table for the month of March and joins it with the `employee` table to get the department information.
- The main query calculates the average salary for each department in March and compares it with the average salary across the company for the same month.
- The `CASE` statement is used to categorize the comparison as 'higher', 'lower', or 'same' based on the department's average salary.
