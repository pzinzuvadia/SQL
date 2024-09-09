
# Contest Leaderboard
[**HackerRank Link**](https://www.hackerrank.com/challenges/contest-leaderboard?isFullScreen=true)

---

## Question Explanation:
The task is to generate a leaderboard that ranks hackers based on their total score in all challenges. The leaderboard should only include hackers who have scored more than zero in at least one challenge. In case of ties in the total score, the hackers should be ranked by their hacker ID in ascending order.

---

## Sample Tables:
The table `hackers` contains the following columns:
- `hacker_id` - Unique identifier for the hacker.
- `name` - Name of the hacker.

The table `submissions` contains the following columns:
- `hacker_id` - Foreign key linking to the `hackers` table.
- `challenge_id` - The identifier for the challenge.
- `score` - The score the hacker received for the submission.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select hacker_id, name, sum(max_score) as tot_score
from (select h.hacker_id as hacker_id, h.name as name, max(s.score) as max_score
      from hackers h
      join submissions s
      on h.hacker_id = s.hacker_id
      group by h.hacker_id, h.name, s.challenge_id) x
group by hacker_id, name
having sum(max_score) > 0
order by tot_score desc, hacker_id asc;
```

---

## Answer Explanation:
1. The inner subquery retrieves the maximum score for each challenge a hacker has submitted to. It groups by `hacker_id`, `name`, and `challenge_id`, selecting the highest score (`max(s.score)`).
2. The outer query sums up the maximum scores for each hacker (`sum(max_score)`), filters out hackers with a total score of 0, and ranks them by total score in descending order.
3. In the case of ties in total scores, the hackers are ranked by their `hacker_id` in ascending order.
