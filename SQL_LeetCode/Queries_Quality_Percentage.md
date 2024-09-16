
# Queries Quality and Percentage

## Question

Write an SQL query to find the **quality** and **poor query percentage** for each query.

The **quality** of a query is defined as the average of `rating/position` for each query.

The **poor query percentage** is defined as the percentage of queries with a rating less than 3.

Return the result table in **any order**.

### Example:

**Queries Table:**

| query_name | position | rating |
|------------|----------|--------|
| Query1     | 1        | 5      |
| Query1     | 2        | 4      |
| Query2     | 1        | 1      |
| Query2     | 2        | 2      |
| Query3     | 1        | 3      |

**Output:**

| query_name | quality | poor_query_percentage |
|------------|---------|-----------------------|
| Query1     | 4.50    | 0.00                  |
| Query2     | 0.75    | 100.00                |
| Query3     | 3.00    | 0.00                  |

### Explanation:

- For `Query1`, the quality is (5/1 + 4/2) / 2 = 4.50, and the poor query percentage is 0% because no query has a rating less than 3.
- For `Query2`, the quality is (1/1 + 2/2) / 2 = 0.75, and the poor query percentage is 100% because both queries have a rating less than 3.
- For `Query3`, the quality is (3/1) = 3.00, and there are no poor queries with a rating less than 3.

---

## Solution

```sql
SELECT query_name, 
       ROUND(AVG(rating/position), 2) AS quality, 
       ROUND((COUNT(CASE WHEN rating < 3 THEN 1 END)/COUNT(rating))*100, 2) AS poor_query_percentage
FROM queries
GROUP BY query_name
HAVING query_name IS NOT NULL;
```

## Solution Explanation

- The query first calculates the **quality** for each query by dividing the `rating` by the `position` and taking the average.
- The **poor query percentage** is calculated by counting the queries with a `rating` less than 3 and dividing it by the total number of queries for that `query_name`.
- The `ROUND` function is used to round both the `quality` and `poor_query_percentage` to two decimal places.
- The `HAVING` clause ensures that only queries with non-null `query_name` values are included in the result.
