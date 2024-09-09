
# Interviews
[**HackerRank Link**](https://www.hackerrank.com/challenges/interviews?isFullScreen=true)

---

## Question Explanation:
The task is to generate a report that shows statistics for each contest, including:
- Total submissions
- Total accepted submissions
- Total views
- Total unique views

The data is grouped by contest, and we are required to return the contest ID, hacker ID, contest name, and the sum of the above statistics.

---

## Sample Tables:
- `contests`: Contains contest details, including `contest_id`, `hacker_id`, and `name`.
- `colleges`: Contains `college_id` and `contest_id`, linking contests to colleges.
- `challenges`: Contains `challenge_id` and `college_id`, linking challenges to colleges.
- `submission_stats`: Contains `challenge_id`, `total_submissions`, and `total_accepted_submissions` for each challenge.
- `view_stats`: Contains `challenge_id`, `total_views`, and `total_unique_views` for each challenge.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select c.contest_id, c.hacker_id, c.name, sum(ss.ts), sum(ss.tas), sum(vs.tv), sum(vs.tuv)
from contests c
join colleges clg
on c.contest_id = clg.contest_id
join challenges chl
on chl.college_id = clg.college_id
left join (
    select challenge_id, sum(total_submissions) as ts, sum(total_accepted_submissions) as tas 
    from submission_stats group by challenge_id) ss
on ss.challenge_id = chl.challenge_id
left join (
    select challenge_id, sum(total_views) as tv, sum(total_unique_views) as tuv 
    from view_stats group by challenge_id) vs
on vs.challenge_id = chl.challenge_id
group by c.contest_id, c.hacker_id, c.name
order by c.contest_id;
```

---

## Answer Explanation:
1. The query retrieves information from multiple tables (`contests`, `colleges`, `challenges`, `submission_stats`, and `view_stats`).
2. It sums up the statistics for submissions, accepted submissions, views, and unique views for each contest.
3. The results are grouped by `contest_id`, `hacker_id`, and `name` to display statistics for each contest.
4. The `LEFT JOIN` ensures that even if some challenges have no submissions or views, they are still included in the final result.
