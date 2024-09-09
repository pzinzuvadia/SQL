
# Harry Potter and Wands
[**HackerRank Link**](https://www.hackerrank.com/challenges/harry-potter-and-wands?isFullScreen=true)

---

## Question Explanation:
The task is to find the wands that are not evil, selecting the cheapest wand for each age and power combination. The results should be ordered by power in descending order and by age in descending order.

---

## Sample Tables:
- `wands`: Contains information about wands, including `id`, `code`, `power`, and `coins_needed`.
- `wands_property`: Contains properties of the wands, including `code`, `age`, and whether the wand is evil (`is_evil`).

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select w.id, wp.age, w.coins_needed, w.power
from wands w
join wands_property wp
on w.code = wp.code
where wp.is_evil = 0 
  and w.coins_needed = (
    select min(a.coins_needed)
    from wands a
    join wands_property b
    on a.code=b.code
    where b.is_evil = 0 
      and b.age = wp.age 
      and w.power = a.power
    group by b.age, a.power
    order by a.power desc, b.age desc
  )
order by w.power desc, wp.age desc;
```

---

## Answer Explanation:
1. The query joins the `wands` and `wands_property` tables based on the `code`.
2. The `WHERE` clause ensures that only non-evil wands are selected (`wp.is_evil = 0`).
3. The subquery finds the minimum `coins_needed` for each combination of `age` and `power`.
4. The `GROUP BY` clause in the subquery groups the wands by age and power.
5. The main query orders the results by `power` and `age` in descending order.
