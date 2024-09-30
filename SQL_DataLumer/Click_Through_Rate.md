
# Click Through Rate

## Question

You are given a table `events` that contains `app_id`, `event_type`, and `timestamp`. Write an SQL query to calculate the click-through rate (CTR) for each app in the year 2022. The click-through rate is calculated as:

\[
CTR = \left( rac{clicks}{impressions} ight) 	imes 100
\]

Return the result with the `app_id` and `ctr` (click-through rate) rounded to two decimal places.

### Example:

**Events Table:**

| app_id | event_type | timestamp          |
|--------|------------|--------------------|
| 1      | click      | 2022-05-01 12:00:00|
| 1      | impression | 2022-05-01 12:01:00|
| 2      | impression | 2022-05-01 12:00:00|
| 1      | click      | 2022-05-02 14:00:00|

**Output:**

| app_id | ctr  |
|--------|------|
| 1      | 100.0|
| 2      | 0.00 |

### Explanation:

- App 1 had 2 clicks and 2 impressions, resulting in a CTR of 100.0%.
- App 2 had 0 clicks and 1 impression, resulting in a CTR of 0.0%.

---

## Solution

```sql
WITH cte1 AS (
    SELECT app_id, 
           COUNT(CASE WHEN event_type = 'click' THEN 1 END) AS clicks,
           COUNT(CASE WHEN event_type = 'impression' THEN 1 END) AS impressions
    FROM events
    WHERE EXTRACT(YEAR FROM timestamp) = '2022'
    GROUP BY app_id
)

SELECT app_id, ROUND(100.0 * clicks / impressions, 2) AS ctr
FROM cte1;
```

## Solution Explanation

- The common table expression (CTE) `cte1` counts the number of `click` and `impression` events for each `app_id` in 2022.
- The outer query calculates the click-through rate (CTR) as the ratio of `clicks` to `impressions`, multiplied by 100, and rounds the result to two decimal places.
