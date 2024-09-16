
# Managers with at least 5 Direct Reports

## Question

Write an SQL query to report the names of all managers who have at least five direct reports.

Return the result table in **any order**.

The query result format is in the following example.

### Example:

**Employee Table:**

| Id | Name    | Department | ManagerId |
|----|---------|------------|-----------|
| 1  | John    | HR         | Null      |
| 2  | Jane    | IT         | 1         |
| 3  | Alice   | IT         | 1         |
| 4  | Bob     | HR         | 2         |
| 5  | Charlie | IT         | 2         |
| 6  | Dan     | HR         | 2         |

**Output:**

| Name  |
|-------|
| Jane  |

### Explanation:

- John is the manager of Jane and Alice. He has 2 direct reports.
- Jane is the manager of Bob, Charlie, and Dan. She has 3 direct reports.

Since Jane has more than 5 direct reports, we include her name in the result table.

---

## Solution

```sql
SELECT m.name
FROM employee e
JOIN employee m
ON e.managerid = m.id
GROUP BY m.id
HAVING COUNT(e.id) >= 5;
```

## Solution Explanation

- We use a **self-join** on the `employee` table where one instance of the table (`m`) represents the manager, and the other (`e`) represents the employee.
- The join condition `e.managerid = m.id` helps us associate each employee with their manager.
- The `GROUP BY` clause groups the data by the manager's ID (`m.id`).
- The `HAVING` clause filters the results to only include managers who have 5 or more direct reports by counting the employees under each manager using `COUNT(e.id) >= 5`.
