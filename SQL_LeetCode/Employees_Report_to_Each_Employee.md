
# The Number of Employees Which Report to Each Employee

## Question

Write an SQL query to find the number of employees who report to each employee, along with the average age of the reporting employees.

The result should include the `employee_id`, `name`, `reports_count`, and `average_age` of the employees who have subordinates.

Return the result table ordered by `employee_id`.

### Example:

**Employees Table:**

| employee_id | name   | age | reports_to |
|-------------|--------|-----|------------|
| 1           | John   | 45  | NULL       |
| 2           | Jane   | 38  | 1          |
| 3           | Alice  | 30  | 1          |
| 4           | Bob    | 28  | 2          |
| 5           | Charlie| 25  | 2          |

**Output:**

| employee_id | name   | reports_count | average_age |
|-------------|--------|---------------|-------------|
| 1           | John   | 2             | 34          |
| 2           | Jane   | 2             | 27          |

### Explanation:

- John has two employees reporting to him: Jane (age 38) and Alice (age 30), so his `reports_count` is 2, and the `average_age` is (38 + 30) / 2 = 34.
- Jane has two employees reporting to her: Bob (age 28) and Charlie (age 25), so her `reports_count` is 2, and the `average_age` is (28 + 25) / 2 = 27.

---

## Solution

```sql
SELECT m.employee_id, m.name, COUNT(e.employee_id) AS reports_count, ROUND(AVG(e.age), 0) AS average_age
FROM employees e
JOIN employees m
ON e.reports_to = m.employee_id 
GROUP BY m.employee_id, m.name
ORDER BY m.employee_id;
```

## Solution Explanation

- The query uses a **self-join** on the `employees` table to associate each employee (`e`) with their manager (`m`).
- The `COUNT` function calculates the number of employees who report to each manager.
- The `AVG` function calculates the average age of the reporting employees, and the result is rounded to the nearest integer using `ROUND`.
- The `GROUP BY` groups the results by `employee_id` and `name`, and the `ORDER BY` ensures the result is ordered by `employee_id`.
