
# Placements
[**HackerRank Link**](https://www.hackerrank.com/challenges/placements?isFullScreen=true)

---

## Question Explanation:
The task is to find students whose friends have secured a higher salary than themselves. We are required to display the names of such students, ordered by their friends' salaries.

---

## Sample Tables:
- `students`: Contains the `id` and `name` of each student.
- `packages`: Contains the `id` of the student and their `salary`.
- `friends`: Contains `id` and `friend_id`, representing friendships between students.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
with cte1(id, name, salary) as (
    select s.id, s.name, p.salary
    from students s
    join packages p
    on s.id = p.id
),
cte2(id, friend_id, friend_salary) as (
    select f.id, f.friend_id, p.salary
    from friends f
    join packages p
    on f.friend_id = p.id
)
select cte1.name
from cte1 
join cte2
on cte1.id = cte2.id
where cte1.salary < cte2.friend_salary
order by cte2.friend_salary;
```

---

## Answer Explanation:
1. **CTE 1 (cte1):** Selects each student's `id`, `name`, and `salary` from the `students` and `packages` tables.
2. **CTE 2 (cte2):** Selects the `id` of the student and the `friend_id`, along with the `friend_salary` from the `friends` and `packages` tables.
3. The main query compares the salaries of each student and their friends, displaying only the students whose friends earn more than them.
4. The result is ordered by `friend_salary` in ascending order.
