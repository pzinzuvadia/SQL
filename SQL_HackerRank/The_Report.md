
# The Report
[**HackerRank Link**](https://www.hackerrank.com/challenges/the-report?isFullScreen=true)

---

## Question Explanation:
The task is to generate a report that displays students' names, their grades, and marks. However, only students with a grade of 8 or higher should have their names displayed. The results should be ordered by grade in descending order, followed by the student's name in ascending order and their marks.

---

## Sample Tables:
- `students`: Contains the `name` and `marks` of each student.
- `grades`: Contains `grade`, `min_mark`, and `max_mark` for each grade range.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select case when g.grade >= 8 then s.name end as name, g.grade, s.marks
from students s 
left join grades g
on s.marks between g.min_mark and g.max_mark
order by g.grade desc, s.name asc, s.marks;
```

---

## Answer Explanation:
1. The `CASE` statement ensures that the `name` is displayed only if the student's grade is 8 or higher.
2. The `LEFT JOIN` is used to match students' marks with their respective grades by checking if the marks fall between the `min_mark` and `max_mark` for each grade.
3. The result is ordered by `grade` in descending order, followed by `name` in ascending order, and finally by `marks`.
