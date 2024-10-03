
# Well Paid Employees

## Question

You are given a table `employee` that contains `employee_id`, `name`, `salary`, and `manager_id`. Write an SQL query to find the employees who earn more than their managers.

Return the result with the `employee_id` and `name` of the employees who earn more than their managers.

### Example:

**Employee Table:**

| employee_id | name   | salary | manager_id |
|-------------|--------|--------|------------|
| 1           | John   | 100    | NULL       |
| 2           | Jane   | 200    | 1          |
| 3           | Doe    | 150    | 1          |

**Output:**

| employee_id | name   |
|-------------|--------|
| 2           | Jane   |

### Explanation:

- Jane earns 200, which is more than her manager (John), who earns 100.

---

## Solution

```sql
SELECT e.employee_id, e.name
FROM employee e  
JOIN employee m  
ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

## Solution Explanation

- The query joins the `employee` table with itself to compare the salaries of employees with their managers.
- It filters for rows where the employee's salary (`e.salary`) is greater than the manager's salary (`m.salary`).
- The result contains the `employee_id` and `name` of employees who earn more than their managers.
