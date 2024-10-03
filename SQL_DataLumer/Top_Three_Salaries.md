
# SQL Top Three Salaries

## Question

You are given two tables: `employee` and `department`. The `employee` table contains `name`, `salary`, and `department_id`, and the `department` table contains `department_id` and `department_name`. Write an SQL query to find the top three highest-paid employees in each department.

Return the result with the `department_name`, `name`, and `salary` of the top three highest-paid employees, ordered by salary in descending order within each department.

### Example:

**Employee Table:**

| name   | salary | department_id |
|--------|--------|---------------|
| John   | 100    | 1             |
| Jane   | 200    | 1             |
| Doe    | 150    | 1             |
| Mike   | 80     | 2             |
| Mary   | 120    | 2             |

**Department Table:**

| department_id | department_name |
|---------------|-----------------|
| 1             | HR              |
| 2             | IT              |

**Output:**

| department_name | name   | salary |
|-----------------|--------|--------|
| HR              | Jane   | 200    |
| HR              | Doe    | 150    |
| HR              | John   | 100    |
| IT              | Mary   | 120    |
| IT              | Mike   | 80     |

---

## Solution

```sql
WITH cte1 AS (
    SELECT d.department_name, e.name, e.salary, 
           DENSE_RANK() OVER (PARTITION BY d.department_name ORDER BY salary DESC) AS rnk
    FROM employee e  
    JOIN department d  
    ON e.department_id = d.department_id
)

SELECT department_name, name, salary
FROM cte1
WHERE rnk <= 3;
```

## Solution Explanation

- The common table expression (CTE) `cte1` calculates the rank (`rnk`) of each employee's salary within their department using the `DENSE_RANK()` function, partitioning by `department_name`.
- The outer query filters the result to include only the top three employees (`rnk <= 3`) in each department.
