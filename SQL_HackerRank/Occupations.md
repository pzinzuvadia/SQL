
# Occupations
[**HackerRank Link**](https://www.hackerrank.com/challenges/occupations?isFullScreen=true)

---

## Question Explanation:
The task is to pivot occupation data so that the names of people in each occupation are displayed in separate columns (Doctor, Professor, Singer, and Actor). The names should be sorted alphabetically within each occupation, and the rows should be ordered by the number of entries in each occupation.

---

## Sample Table:
The table `occupations` contains the following columns:
- `name` - Name of the person.
- `occupation` - The occupation of the person (Doctor, Professor, Singer, Actor).

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select min(case when x.occupation = 'Doctor' then x.name end) as Doctor,
    min(case when x.occupation = 'Professor' then x.name end) as Professor,
    min(case when x.occupation = 'Singer' then x.name end) as Singer,
    min(case when x.occupation = 'Actor' then x.name end) as Actor
from (select name, occupation, row_number() over(partition by occupation order by name) as rn from occupations) x
group by x.rn;
```

---

## Answer Explanation:
1. The subquery generates a row number for each name within their respective occupation, sorted alphabetically.
2. The outer query uses the `CASE` statement to pivot the data and select the names based on their occupations.
3. The `MIN` function is used to ensure each row in the result contains the correct name for each occupation, and the `GROUP BY x.rn` ensures that the results are grouped by the generated row numbers.
