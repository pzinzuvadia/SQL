
# The PADS
[**HackerRank Link**](https://www.hackerrank.com/challenges/the-pads?isFullScreen=true)

---

## Question Explanation:
The task requires two parts:
1. Display the names of people from the `occupations` table along with the first letter of their occupation in parentheses.
2. For each occupation, display the total number of people with that occupation in the format: "There are a total of X occupation(s)."

---

## Sample Table:
- `occupations`: Contains `name` and `occupation` for each individual.

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
-- Part 1: Display names with the first letter of their occupation
select concat(name, '(', substr(occupation, 1, 1), ')')
from occupations
order by name;

-- Part 2: Display total counts of each occupation
select concat('There are a total of ', count(name), ' ', lower(occupation), 's.')
from occupations
group by occupation
order by count(name);
```

---

## Answer Explanation:
1. **Part 1:** The `concat` function is used to combine the `name` with the first letter of their `occupation`, which is extracted using the `substr` function. The result is ordered by `name`.
2. **Part 2:** The `count` function counts the number of people for each occupation, and the result is formatted with `concat`. The occupations are displayed in descending order of the count of names.
