
# Oracle Sales Quota

## Question

You are given two tables: `deals` and `sales_quotas`. The `deals` table contains `employee_id` and `deal_size`, and the `sales_quotas` table contains `employee_id` and `quota`. Write an SQL query to find out which employees met or exceeded their sales quota.

Return the result with the `employee_id` and a column `made_quota` indicating whether the employee met or exceeded their sales quota ('yes' or 'no'). The result should be ordered by `employee_id`.

### Example:

**Deals Table:**

| employee_id | deal_size |
|-------------|-----------|
| 1           | 100       |
| 1           | 200       |
| 2           | 50        |

**Sales Quotas Table:**

| employee_id | quota |
|-------------|-------|
| 1           | 250   |
| 2           | 100   |

**Output:**

| employee_id | made_quota |
|-------------|------------|
| 1           | yes        |
| 2           | no         |

### Explanation:

- Employee 1 had total deals of 300 and met the quota of 250.
- Employee 2 had total deals of 50 and did not meet the quota of 100.

---

## Solution

```sql
WITH cte1 AS (
    SELECT employee_id, SUM(deal_size) AS total_deal
    FROM deals
    GROUP BY employee_id
)

SELECT c.employee_id, 
       CASE WHEN s.quota <= c.total_deal THEN 'yes' ELSE 'no' END AS made_quota
FROM sales_quotas s  
JOIN cte1 c  
ON c.employee_id = s.employee_id
ORDER BY c.employee_id;
```

## Solution Explanation

- The common table expression (CTE) `cte1` calculates the total deals for each employee by summing the `deal_size` from the `deals` table.
- The outer query joins the `sales_quotas` table with `cte1` to compare the total deals with the sales quota.
- The `CASE` statement is used to return 'yes' if the employee met or exceeded the quota, and 'no' otherwise.
