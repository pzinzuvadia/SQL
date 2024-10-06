
# Top Fans Rank

## Question

You are given three tables: `global_song_rank`, `songs`, and `artists`. The `global_song_rank` table contains `song_id`, `day`, and `rank`. The `songs` table contains `song_id` and `artist_id`, and the `artists` table contains `artist_id` and `artist_name`. Write an SQL query to find the top 5 artists whose songs have appeared in the top 10 rankings the most frequently.

Return the result with `artist_name` and `artist_rank` (the rank of the artist based on the number of top 10 appearances).

### Example:

**Global Song Rank Table:**

| song_id | day       | rank |
|---------|-----------|------|
| 1       | 2023-07-01| 5    |
| 2       | 2023-07-01| 10   |
| 1       | 2023-07-02| 9    |

**Songs Table:**

| song_id | artist_id |
|---------|-----------|
| 1       | 1001      |
| 2       | 1002      |

**Artists Table:**

| artist_id | artist_name |
|-----------|-------------|
| 1001      | Artist A    |
| 1002      | Artist B    |

**Output:**

| artist_name | artist_rank |
|-------------|-------------|
| Artist A    | 1           |
| Artist B    | 2           |

---

## Solution

```sql
WITH cte1 AS (
    SELECT a.artist_name, COUNT(*) AS cnt
    FROM global_song_rank g
    JOIN songs s 
    ON s.song_id = g.song_id
    JOIN artists a  
    ON s.artist_id = a.artist_id
    WHERE g.rank <= 10
    GROUP BY a.artist_name
)

SELECT artist_name, rnk AS artist_rank
FROM (
    SELECT artist_name, cnt, DENSE_RANK() OVER (ORDER BY cnt DESC) AS rnk 
    FROM cte1
) x  
WHERE rnk <= 5;
```

## Solution Explanation

- The common table expression (CTE) `cte1` counts how many times each artist's songs have appeared in the top 10 rankings. This is achieved by joining the `global_song_rank`, `songs`, and `artists` tables and filtering for `rank <= 10`.
- The outer query uses the `DENSE_RANK()` function to rank the artists based on the count of top 10 appearances.
- Finally, the result is filtered to include only the top 5 ranked artists.
