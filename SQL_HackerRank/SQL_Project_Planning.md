
# SQL Project Planning
[**HackerRank Link**](https://www.hackerrank.com/challenges/sql-projects?isFullScreen=true)

---

## Question Explanation:
The task is to determine the minimum end date for each project's start date, such that the start date is before the end date and the start and end dates don't overlap with other projects.

---

## Sample Table:
- `projects`: Contains `start_date` and `end_date` for each project.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select s.start_date, min(e.end_date)
from projects s
join projects e
on (s.start_date not in (select end_date from projects)) 
and (e.end_date not in (select start_date from projects))
where s.start_date < e.end_date
group by s.start_date
order by datediff(min(e.end_date), s.start_date), s.start_date;
```

---

## Answer Explanation:
1. The query joins the `projects` table on itself to find valid start and end date pairs where the start date is less than the end date, and the dates don't overlap with other projects.
2. The `WHERE` clause filters out any overlapping start and end dates.
3. The `DATEDIFF` function is used to order the results by the difference between the start date and the minimum end date, ensuring projects are ordered by the shortest duration first.
