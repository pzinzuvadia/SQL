
# Count Salary Categories

## Question

Write an SQL query to count the number of accounts in each salary category:

- **Low Salary**: Income less than 20,000
- **Average Salary**: Income between 20,000 and 50,000 inclusive
- **High Salary**: Income greater than 50,000

Return the result table in **any order**.

### Example:

**Accounts Table:**

| account_id | income |
|------------|--------|
| 1          | 10000  |
| 2          | 25000  |
| 3          | 55000  |
| 4          | 75000  |

**Output:**

| category       | accounts_count |
|----------------|----------------|
| Low Salary     | 1              |
| Average Salary | 1              |
| High Salary    | 2              |

### Explanation:

- There is 1 account with an income less than 20,000, which falls under the "Low Salary" category.
- There is 1 account with an income between 20,000 and 50,000 inclusive, which falls under the "Average Salary" category.
- There are 2 accounts with an income greater than 50,000, which fall under the "High Salary" category.

---

## Solution

```sql
SELECT 'Low Salary' AS category, COUNT(CASE WHEN income < 20000 THEN 1 END) AS accounts_count
FROM accounts
UNION
SELECT 'Average Salary' AS category, COUNT(CASE WHEN income >= 20000 AND income <= 50000 THEN 1 END) AS accounts_count
FROM accounts
UNION
SELECT 'High Salary' AS category, COUNT(CASE WHEN income > 50000 THEN 1 END) AS accounts_count
FROM accounts;
```

## Solution Explanation

- The query uses `COUNT` with a `CASE` statement to count the number of accounts in each salary category.
- It uses `UNION` to combine the results for the "Low Salary", "Average Salary", and "High Salary" categories into one result set.
