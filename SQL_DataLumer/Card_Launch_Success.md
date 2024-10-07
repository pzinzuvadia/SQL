
# Card Launch Success

## Question

You are given a table `monthly_cards_issued` that contains `issue_month`, `issue_year`, `card_name`, and `issued_amount`. Write an SQL query to find the amount of cards issued in the first month of launch for each card, and return the results ordered by `issued_amount` in descending order.

Return the result with `card_name` and `issued_amount` for each card, ordered by `issued_amount` in descending order.

### Example:

**Monthly Cards Issued Table:**

| issue_month | issue_year | card_name | issued_amount |
|-------------|------------|-----------|---------------|
| 1           | 2022       | Gold      | 1000          |
| 2           | 2022       | Gold      | 500           |
| 1           | 2022       | Silver    | 200           |

**Output:**

| card_name | issued_amount |
|-----------|---------------|
| Gold      | 1000          |
| Silver    | 200           |

---

## Solution

```sql
SELECT card_name, issued_amount
FROM (
    SELECT issue_month, issue_year, card_name, issued_amount, 
           ROW_NUMBER() OVER (PARTITION BY card_name ORDER BY issue_year, issue_month) AS rnk 
    FROM monthly_cards_issued
) x  
WHERE rnk = 1
ORDER BY issued_amount DESC;
```

## Solution Explanation

- The query uses a common table expression (CTE) to rank each card's issuance by `issue_year` and `issue_month` using the `ROW_NUMBER()` function. This assigns a rank of 1 to the first month of card issuance.
- The main query filters the result to include only the first issuance for each card (`rnk = 1`) and orders the result by `issued_amount` in descending order.
