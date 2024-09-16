
# Monthly Transactions I

## Question

Write an SQL query to find the total number of transactions, the total number of approved transactions, the total transaction amount, and the total approved transaction amount for each country and each month.

The result should be formatted as follows:

- The month should be in the format 'YYYY-MM'.
- The `trans_count` is the total number of transactions in the month.
- The `approved_count` is the number of transactions that were approved.
- The `trans_total_amount` is the total amount of all transactions in the month.
- The `approved_total_amount` is the total amount of approved transactions in the month.

Return the result table in **any order**.

### Example:

**Transactions Table:**

| id | trans_date | country | state    | amount |
|----|------------|---------|----------|--------|
| 1  | 2023-03-15 | US      | approved | 100    |
| 2  | 2023-03-15 | US      | pending  | 200    |
| 3  | 2023-03-15 | CAN     | approved | 150    |
| 4  | 2023-04-01 | US      | approved | 300    |
| 5  | 2023-04-01 | US      | failed   | 50     |

**Output:**

| month     | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
|-----------|---------|-------------|----------------|--------------------|-----------------------|
| 2023-03   | US      | 2           | 1              | 300                | 100                   |
| 2023-03   | CAN     | 1           | 1              | 150                | 150                   |
| 2023-04   | US      | 2           | 1              | 350                | 300                   |

### Explanation:

- In March 2023, there were 2 transactions in the US, of which 1 was approved, and the total transaction amount was 300, with an approved total amount of 100.
- In March 2023, there was 1 transaction in Canada, which was approved, and the total transaction amount was 150, with an approved total amount of 150.
- In April 2023, there were 2 transactions in the US, of which 1 was approved, and the total transaction amount was 350, with an approved total amount of 300.

---

## Solution

```sql
SELECT 
    CONCAT(YEAR(trans_date), '-', DATE_FORMAT(trans_date, '%m')) AS month, 
    country, 
    COUNT(id) AS trans_count, 
    COUNT(CASE WHEN state = 'approved' THEN 1 END) AS approved_count, 
    SUM(amount) AS trans_total_amount, 
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM transactions
GROUP BY CONCAT(YEAR(trans_date), '-', MONTH(trans_date)), country;
```

## Solution Explanation

- The `CONCAT` and `DATE_FORMAT` functions are used to format the `trans_date` into the desired 'YYYY-MM' format.
- The `COUNT` function counts the total number of transactions for each month and country, and the `COUNT` with the `CASE` statement counts the number of approved transactions.
- The `SUM` function calculates the total transaction amount, while the `SUM` with the `CASE` statement calculates the total amount of approved transactions.
- The `GROUP BY` groups the results by the formatted month and the `country`.
