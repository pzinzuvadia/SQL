
# International Call Percentage

## Question

You are given two tables: `phone_calls` and `phone_info`. The `phone_calls` table contains `caller_id` and `receiver_id`, and the `phone_info` table contains `caller_id` and `country_id`. Write an SQL query to calculate the percentage of international calls, where an international call is defined as a call made between two different countries.

Return the result as the percentage of international calls, rounded to one decimal place.

### Example:

**Phone Calls Table:**

| caller_id | receiver_id |
|-----------|-------------|
| 1         | 2           |
| 3         | 4           |

**Phone Info Table:**

| caller_id | country_id |
|-----------|------------|
| 1         | US         |
| 2         | CA         |
| 3         | US         |
| 4         | US         |

**Output:**

| international_call_percentage |
|-------------------------------|
| 50.0                          |

### Explanation:

- 50% of the calls were international, as one call was made between the US and Canada.

---

## Solution

```sql
WITH cte1 AS (
    SELECT pc.caller_id, 
           (SELECT country_id FROM phone_info pi WHERE pi.caller_id = pc.caller_id) AS caller_country,
           pc.receiver_id, 
           (SELECT country_id FROM phone_info pi WHERE pi.caller_id = pc.receiver_id) AS receiver_country
    FROM phone_calls pc
)

SELECT ROUND(100.0 * COUNT(CASE WHEN caller_country != receiver_country THEN 1 END) / COUNT(caller_id), 1) 
AS international_call_percentage
FROM cte1;
```

## Solution Explanation

- The common table expression (CTE) `cte1` retrieves the country of both the caller and receiver by joining the `phone_calls` and `phone_info` tables.
- The main query calculates the percentage of international calls by counting the number of calls where the caller and receiver are in different countries and dividing it by the total number of calls.
- The result is rounded to one decimal place using the `ROUND()` function.
