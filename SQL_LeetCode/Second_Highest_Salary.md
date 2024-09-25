
# Second Highest Salary

## Question

Write an SQL query to find the second highest salary from the `employee` table. If there is no second highest salary, return `null`.

Return the result as a column named `SecondHighestSalary`.

### Example:

**Employee Table:**

| id | salary |
|----|--------|
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |

**Output:**

| SecondHighestSalary |
|---------------------|
| 200                 |

### Explanation:

- The highest salary is 300, and the second highest salary is 200.

If the table only contains one row, the output should be `null`.

---

## Solution

```sql
SELECT (
    SELECT DISTINCT salary
    FROM employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

## Solution Explanation

- The subquery selects the distinct `salary` values from the `employee` table, orders them in descending order, and uses `OFFSET 1` to skip the highest salary and `LIMIT 1` to return the second highest salary.
- If no second highest salary exists, the result will be `null`.
