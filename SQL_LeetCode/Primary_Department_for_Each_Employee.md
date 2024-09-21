
# Primary Department for Each Employee

## Question

Write an SQL query to find the primary department for each employee. If an employee is associated with only one department, return that department. If an employee is associated with multiple departments, return the department where the `primary_flag` is set to 'Y'.

Return the result table in **any order**.

### Example:

**Employee Table:**

| employee_id | department_id | primary_flag |
|-------------|---------------|--------------|
| 1           | 101           | Y            |
| 2           | 102           | N            |
| 2           | 103           | Y            |
| 3           | 104           | N            |

**Output:**

| employee_id | department_id |
|-------------|---------------|
| 1           | 101           |
| 2           | 103           |
| 3           | 104           |

### Explanation:

- Employee 1 is associated with only one department, so department 101 is returned.
- Employee 2 is associated with two departments, and department 103 has the primary flag set to 'Y', so it is returned.
- Employee 3 is associated with only one department, so department 104 is returned.

---

## Solution

```sql
SELECT employee_id, department_id
FROM (
    SELECT employee_id, department_id, primary_flag, COUNT(primary_flag) OVER(PARTITION BY employee_id) AS cnt
    FROM employee
) x
WHERE cnt = 1 OR primary_flag = 'Y';
```

## Solution Explanation

- The query uses the `COUNT` window function to count the number of departments associated with each employee. This is done using the `PARTITION BY` clause, which groups rows by `employee_id`.
- The `WHERE` clause filters the result to include only those employees who are associated with a single department or where the `primary_flag` is set to 'Y'.
