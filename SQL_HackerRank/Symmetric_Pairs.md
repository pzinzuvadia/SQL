
# Symmetric Pairs
[**HackerRank Link**](https://www.hackerrank.com/challenges/symmetric-pairs?isFullScreen=true)

---

## Question Explanation:
The task is to find all symmetric pairs `(x, y)` in a table, where a symmetric pair means that for a pair `(x, y)`, there is another pair `(y, x)` in the same table. The pairs should be displayed in ascending order by `x`.

---

## Sample Table:
- `functions`: Contains pairs of values `x` and `y`.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select f1.x, f1.y
from functions f1
join functions f2
on f1.x = f2.y and f1.y = f2.x
where f1.x <= f1.y
group by f1.x, f1.y, f2.x, f2.y
having count(*) = 4 or f1.x != f1.y
order by f1.x;
```

---

## Answer Explanation:
1. The query joins the `functions` table on itself (`f1` and `f2`), matching pairs `(x, y)` with their symmetric counterparts `(y, x)`.
2. The `WHERE` clause ensures that only pairs where `x <= y` are considered, avoiding duplicate output.
3. The `HAVING` clause ensures that both symmetric pairs are present, with a count of 4 for symmetric pairs or ensuring that `x` and `y` are not the same.
4. The result is ordered by `x` in ascending order.
