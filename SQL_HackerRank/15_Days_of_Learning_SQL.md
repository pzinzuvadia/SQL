
# 15 Days of Learning SQL
[**HackerRank Link**](https://www.hackerrank.com/challenges/15-days-of-learning-sql/problem?isFullScreen=true)

---

## Question Explanation:
The task is to analyze hacker submissions over 15 days, compute the number of distinct hackers who made submissions each day, and determine the hacker with the most submissions for each day. If two hackers have the same number of submissions, the one with the lower `hacker_id` is prioritized.

---

## Sample Tables:
- `submissions`: Contains `submission_date`, `hacker_id`, and other details about submissions.
- `hackers`: Contains `hacker_id` and `name`.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
with MaxSubEachDay as (
    select submission_date,
           hacker_id,
           RANK() OVER(partition by submission_date order by SubCount desc, hacker_id) as Rn
    FROM
    (select submission_date, hacker_id, count(1) as SubCount 
     from submissions
     group by submission_date, hacker_id
     ) subQuery
), DayWiseRank as (
    select submission_date,
           hacker_id,
           DENSE_RANK() OVER(order by submission_date) as dayRn
    from submissions
), HackerCntTillDate as (
    select outtr.submission_date,
           outtr.hacker_id,
           case when outtr.submission_date = '2016-03-01' then 1
                else 1 + (select count(distinct a.submission_date) 
                          from submissions a 
                          where a.hacker_id = outtr.hacker_id and a.submission_date < outtr.submission_date)
            end as PrevCnt,
           outtr.dayRn
    from DayWiseRank outtr
), HackerSubEachDay as (
    select submission_date,
           count(distinct hacker_id) as HackerCnt
    from HackerCntTillDate
    where PrevCnt = dayRn
    group by submission_date
)
select HackerSubEachDay.submission_date,
       HackerSubEachDay.HackerCnt,
       MaxSubEachDay.hacker_id,
       Hackers.name
from HackerSubEachDay
inner join MaxSubEachDay
    on HackerSubEachDay.submission_date = MaxSubEachDay.submission_date
inner join Hackers
    on Hackers.hacker_id = MaxSubEachDay.hacker_id
where MaxSubEachDay.Rn = 1;
```

---

## Answer Explanation:
1. **MaxSubEachDay:** Calculates the rank of each hacker based on their submission count for each day.
2. **DayWiseRank:** Assigns a rank to each submission date for hackers.
3. **HackerCntTillDate:** Counts the number of distinct submission days for each hacker.
4. **HackerSubEachDay:** Counts the distinct hackers who made submissions on each day.
5. The main query joins the results to display the submission date, the number of hackers, and the top hacker's name and `hacker_id`.
