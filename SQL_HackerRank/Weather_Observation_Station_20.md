
# Weather Observation Station 20
[**HackerRank Link**](https://www.hackerrank.com/challenges/weather-observation-station-20?isFullScreen=true)

---

## Question Explanation:
The task is to calculate the median value of the `lat_n` (latitude) values from the `station` table. If there are an odd number of records, the median is the middle value. If there are an even number of records, the median is the average of the two middle values. The result should be rounded to 4 decimal places.

---

## Sample Table:
- `station`: Contains the following column:
  - `lat_n`: The latitude.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
with x as (
    select lat_n, row_number() over(order by lat_n) as rn, count(lat_n) over() as cnt
    from station
)
select round(min(case when cnt%2 != 0 then (select lat_n from x where rn = ceil(cnt/2))
else
((select lat_n from x where rn = cnt/2) + (select lat_n from x where rn = cnt/2 + 1))/2 
end), 4) as median
from x;
```

---

## Answer Explanation:
1. The query calculates the row number (`rn`) for each `lat_n` value, ordering by latitude, and also computes the total count (`cnt`) of `lat_n` values.
2. The `CASE` statement handles both cases:
   - If there are an odd number of records, the median is the middle value.
   - If there are an even number of records, the median is the average of the two middle values.
3. The `round` function ensures the result is rounded to 4 decimal places.
