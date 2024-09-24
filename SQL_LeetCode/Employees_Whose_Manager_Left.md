
# Employees Whose Manager Left the Company

## Question

Write an SQL query to find the employees whose manager has left the company. Employees have a salary of less than 30,000 and have a manager (`manager_id` is not null), but their manager is no longer listed as an employee.

Return the result table ordered by `employee_id`.

### Example:

**Employees Table:**

| employee_id | manager_id | salary |
|-------------|------------|--------|
| 1           | 2          | 20000  |
| 2           | NULL       | 50000  |
| 3           | 2          | 25000  |
| 4           | 1          | 30000  |
| 5           | 1          | 25000  |
| 6           | 5          | 25000  |

**Output:**

| employee_id |
|-------------|
| 1           |
| 6           |

### Explanation:

- Employee 1 has a manager (manager_id = 2), but manager 2 is not listed in the employees table, meaning the manager has left.
- Employee 6 has a manager (manager_id = 5), but manager 5 is no longer listed in the employees table.

---

## Solution

```sql
SELECT employee_id
FROM employees
WHERE salary < 30000 AND manager_id NOT IN (SELECT employee_id FROM employees)
ORDER BY employee_id;
```

## Solution Explanation

- The query selects employees with a salary less than 30,000 and filters out those whose `manager_id` is not found in the list of current employees (`employee_id`), indicating that their manager has left the company.
