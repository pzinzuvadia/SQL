
# Matching Skills

## Question

You are given a table `candidates` that contains the `candidate_id` and their corresponding skills. Write an SQL query to find the candidates who possess all three of the following skills: `Python`, `Tableau`, and `PostgreSQL`.

Return the result as a list of `candidate_id` sorted in ascending order.

### Example:

**Candidates Table:**

| candidate_id | skill      |
|--------------|------------|
| 1            | Python     |
| 1            | Tableau    |
| 1            | PostgreSQL |
| 2            | Python     |
| 2            | Tableau    |
| 3            | Python     |
| 3            | PostgreSQL |

**Output:**

| candidate_id |
|--------------|
| 1            |

### Explanation:

- Candidate 1 has all three required skills (Python, Tableau, PostgreSQL).
- Candidate 2 is missing PostgreSQL.
- Candidate 3 is missing Tableau.

---

## Solution

```sql
SELECT candidate_id
FROM (
    SELECT candidate_id, skill
    FROM candidates
    WHERE skill IN ('Python', 'Tableau', 'PostgreSQL')
) x
GROUP BY candidate_id
HAVING COUNT(DISTINCT skill) = 3
ORDER BY candidate_id;
```

## Solution Explanation

- The inner query filters the `candidates` table to only include rows where the skill is one of the three required skills (Python, Tableau, PostgreSQL).
- The outer query groups the results by `candidate_id` and counts the distinct skills for each candidate.
- The `HAVING COUNT(DISTINCT skill) = 3` clause ensures that only candidates who possess all three skills are included in the result.
- The result is ordered by `candidate_id`.
