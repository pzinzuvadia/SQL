
# Print Prime Numbers
[**HackerRank Link**](https://www.hackerrank.com/challenges/print-prime-numbers?isFullScreen=true)

---

## Question Explanation:
The task is to generate a list of prime numbers between 1 and 1000. The output should be a single string of prime numbers, concatenated with `&` as the separator.

---

## Sample Table:
No specific tables are provided, as this problem involves generating numbers between 1 and 1000 and determining whether they are prime.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
with recursive cte1 as (
    select 1 as n
    union all
    select n+1
    from cte1
    where n+1 <= 1000
),
cte2 as (
    select t1.n
    from cte1 t1
    join cte1 t2
    where t1.n >= t2.n and t1.n % t2.n = 0
    group by t1.n
    having count(t2.n) = 2
)
select group_concat(n order by n separator '&')
from cte2;
```

---

## Answer Explanation:
1. **CTE 1 (cte1):** Recursively generates numbers from 1 to 1000.
2. **CTE 2 (cte2):** Identifies prime numbers by checking that each number `n` is divisible by exactly two numbers: 1 and `n` itself.
3. The main query uses `group_concat` to concatenate the prime numbers into a single string, with `&` as the separator.
