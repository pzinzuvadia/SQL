
# Weather Observation Station 19
[**HackerRank Link**](https://www.hackerrank.com/challenges/weather-observation-station-19?isFullScreen=true)

---

## Question Explanation:
The task is to calculate the Euclidean distance between the maximum and minimum latitude (`lat_n`) and longitude (`long_w`) values from the `station` table. The result should be rounded to 4 decimal places.

---

## Sample Table:
- `station`: Contains the following columns:
  - `lat_n`: The latitude.
  - `long_w`: The longitude.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select round(sqrt(power((max(lat_n) - min(lat_n)), 2) + power((max(long_w) - min(long_w)), 2)), 4)
from station;
```

---

## Answer Explanation:
1. The `power` function calculates the square of the differences between the maximum and minimum latitude and longitude values.
2. The `sqrt` function calculates the square root of the sum of these squared differences, yielding the Euclidean distance.
3. The `round` function ensures the result is rounded to 4 decimal places.
