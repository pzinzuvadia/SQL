
# Weather Observation Station 18
[**HackerRank Link**](https://www.hackerrank.com/challenges/weather-observation-station-18?isFullScreen=true)

---

## Question Explanation:
The task is to calculate the Manhattan distance between the maximum and minimum latitude (`lat_n`) and longitude (`long_w`) values from the `station` table. The result should be rounded to 4 decimal places.

---

## Sample Table:
- `station`: Contains the following columns:
  - `lat_n`: The latitude.
  - `long_w`: The longitude.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select round(abs(max(lat_n) - min(lat_n)) + abs(max(long_w) - min(long_w)), 4)
from station;
```

---

## Answer Explanation:
1. The `abs` function calculates the absolute difference between the maximum and minimum latitude and longitude values.
2. The Manhattan distance is the sum of these absolute differences.
3. The `round` function ensures the result is rounded to 4 decimal places.
