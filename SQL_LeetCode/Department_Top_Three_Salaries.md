
# Department Top Three Salaries

## Question

Write an SQL query to find the top three salaries for each department.

Return the result table with the department name, employee name, and salary, sorted by department and salary.

### Example:

**Employee Table:**

| id | name   | salary | departmentid |
|----|--------|--------|--------------|
| 1  | Alice  | 60000  | 1            |
| 2  | Bob    | 70000  | 1            |
| 3  | Charlie| 80000  | 1            |
| 4  | David  | 90000  | 2            |
| 5  | Eve    | 100000 | 2            |

**Department Table:**

| id | name      |
|----|-----------|
| 1  | HR        |
| 2  | Engineering |

**Output:**

| department   | employee | salary |
|--------------|----------|--------|
| HR           | Charlie  | 80000  |
| HR           | Bob      | 70000  |
| HR           | Alice    | 60000  |
| Engineering  | Eve      | 100000 |
| Engineering  | David    | 90000  |

### Explanation:

- For the HR department, the top three salaries are 80000, 70000, and 60000.
- For the Engineering department, the top two salaries are 100000 and 90000 (there are fewer than three employees).

---

## Solution

```sql
WITH cte1 AS (
    SELECT id, name, salary, departmentid, DENSE_RANK() OVER(PARTITION BY departmentid ORDER BY salary DESC) AS rnk
    FROM employee
)

SELECT d.name AS department, c.name AS employee, c.salary AS salary
FROM cte1 c
JOIN department d
ON c.departmentid = d.id
WHERE c.rnk <= 3;
```

## Solution Explanation

- The common table expression (CTE) `cte1` ranks employees within each department by salary using the `DENSE_RANK` window function.
- The main query selects the department name, employee name, and salary for employees whose rank is less than or equal to 3.
