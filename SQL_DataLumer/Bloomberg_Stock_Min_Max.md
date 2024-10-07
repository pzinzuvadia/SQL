
# Bloomberg Stock Min Max

## Question

You are given a table `stock_prices` that contains `ticker`, `date`, and `open` (the opening price of the stock on that date). Write an SQL query to find the month with the highest opening price and the month with the lowest opening price for each stock ticker.

Return the result with `ticker`, `highest_mth` (the month with the highest opening price), `highest_open`, `lowest_mth` (the month with the lowest opening price), and `lowest_open`, ordered by `ticker`.

### Example:

**Stock Prices Table:**

| ticker | date       | open |
|--------|------------|------|
| AAPL   | 2023-01-01 | 150  |
| AAPL   | 2023-02-01 | 170  |
| TSLA   | 2023-01-01 | 300  |

**Output:**

| ticker | highest_mth | highest_open | lowest_mth | lowest_open |
|--------|-------------|--------------|------------|-------------|
| AAPL   | Feb-2023    | 170          | Jan-2023   | 150         |
| TSLA   | Jan-2023    | 300          | Jan-2023   | 300         |

---

## Solution

```sql
WITH cte1 AS (
    SELECT ticker, 
           mth AS lowest_mth, 
           open AS lowest_open
    FROM (
        SELECT ticker, 
               CONCAT(TO_CHAR(date, 'Mon'), '-', EXTRACT(YEAR FROM date)) AS mth, 
               open, 
               RANK() OVER (PARTITION BY ticker ORDER BY open ASC) AS rnk
        FROM stock_prices
    ) x  
    WHERE rnk = 1
),

cte2 AS (
    SELECT ticker, 
           mth AS highest_mth, 
           open AS highest_open
    FROM (
        SELECT ticker, 
               CONCAT(TO_CHAR(date, 'Mon'), '-', EXTRACT(YEAR FROM date)) AS mth, 
               open, 
               RANK() OVER (PARTITION BY ticker ORDER BY open DESC) AS rnk
        FROM stock_prices
    ) y 
    WHERE rnk = 1
)

SELECT c1.ticker, 
       c2.highest_mth, 
       c2.highest_open, 
       c1.lowest_mth, 
       c1.lowest_open
FROM cte1 c1
JOIN cte2 c2
ON c1.ticker = c2.ticker;
```

## Solution Explanation

- The first common table expression (CTE) `cte1` calculates the month and opening price for the lowest opening price for each stock ticker.
- The second CTE `cte2` calculates the month and opening price for the highest opening price for each stock ticker.
- The main query joins the two CTEs to return the ticker, highest and lowest months, and their respective opening prices for each stock.
