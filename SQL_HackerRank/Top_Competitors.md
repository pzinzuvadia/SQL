
# Top Competitors
[**HackerRank Link**](https://www.hackerrank.com/challenges/full-score?isFullScreen=true)

---

## Question Explanation:
The task is to find hackers who have achieved a full score in more than one challenge. The output should display the `hacker_id` and `name` of such hackers, sorted by the number of challenges in which they scored full marks, in descending order. If two hackers have the same number of full scores, their `hacker_id` should be used to order them in ascending order.

---

## Sample Tables:
- `hackers`: Contains `hacker_id` and `name` of the hackers.
- `submissions`: Contains `hacker_id`, `challenge_id`, and `score` for each submission.
- `challenges`: Contains `challenge_id` and `difficulty_level` for each challenge.
- `difficulty`: Contains `difficulty_level` and `score` (the full score for that level).

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select h.hacker_id, h.name
from hackers h
join submissions s
on h.hacker_id = s.hacker_id
join challenges chl
on chl.challenge_id = s.challenge_id
join difficulty d
on d.difficulty_level = chl.difficulty_level
where s.score = d.score
group by h.hacker_id, h.name
having count(distinct s.challenge_id) > 1
order by count(distinct s.challenge_id) desc, h.hacker_id asc;
```

---

## Answer Explanation:
1. The query joins the `hackers`, `submissions`, `challenges`, and `difficulty` tables to find hackers who have achieved a full score (`s.score = d.score`) on challenges.
2. It groups the results by `hacker_id` and `name`, ensuring that only hackers who have achieved full scores in more than one challenge are included (`having count(distinct s.challenge_id) > 1`).
3. The results are ordered by the number of full-score challenges in descending order, with ties broken by `hacker_id` in ascending order.
