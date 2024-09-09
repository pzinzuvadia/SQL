
# Challenges
[**HackerRank Link**](https://www.hackerrank.com/challenges/challenges?isFullScreen=true)

---

## Question Explanation:
The task is to retrieve information about hackers and the number of challenges they have completed. The query needs to return details only for hackers who have either:
1. Completed the most challenges.
2. Have completed a number of challenges that is not unique (i.e., another hacker has completed the same number of challenges).

---

## Sample Tables:
The table `hackers` contains the following columns:
- `hacker_id` - Unique identifier for the hacker.
- `name` - Name of the hacker.

The table `challenges` contains the following columns:
- `challenge_id` - Unique identifier for the challenge.
- `hacker_id` - Foreign key linking to the `hackers` table.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
with x(hacker_id, name, cnt, rn) as (
    select h.hacker_id, h.name, count(distinct chl.challenge_id) as cnt, 
           row_number() over(partition by count(distinct chl.challenge_id) order by h.hacker_id) as rn
    from hackers h
    join challenges chl
    on h.hacker_id = chl.hacker_id
    group by h.hacker_id, h.name
    order by count(distinct chl.challenge_id) desc, h.hacker_id asc
)

select hacker_id, name, cnt
from x
where cnt in (
    (select cnt from x group by cnt 
     having cnt = (select max(cnt) from x) or count(hacker_id) < 2)
)
```

---

## Answer Explanation:
1. The `WITH` clause creates a common table expression (CTE) `x` that calculates the number of challenges each hacker has completed (`cnt`). It also assigns a row number (`rn`) to each hacker partitioned by the number of challenges completed.
2. The main `SELECT` query retrieves the hackers who have either completed the most challenges or have the same number of challenges as other hackers.
3. The `WHERE` clause filters the hackers based on the conditions mentioned: hackers who completed the most challenges or those with non-unique challenge counts.
