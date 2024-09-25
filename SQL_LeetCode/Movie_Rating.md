
# Movie Rating

## Question

Write an SQL query to return the following:

1. The name of the user who rated the highest number of distinct movies.
2. The title of the movie that was rated the highest in February 2020.

Return the result table with a column `results`.

### Example:

**MovieRating Table:**

| movie_id | user_id | rating | created_at |
|----------|---------|--------|------------|
| 1        | 1       | 5      | 2020-02-10 |
| 2        | 1       | 4      | 2020-02-10 |
| 1        | 2       | 3      | 2020-02-12 |
| 2        | 2       | 5      | 2020-02-14 |
| 3        | 1       | 4      | 2020-03-01 |

**Users Table:**

| user_id | name   |
|---------|--------|
| 1       | Alice  |
| 2       | Bob    |

**Movies Table:**

| movie_id | title       |
|----------|-------------|
| 1        | Avengers    |
| 2        | Titanic     |
| 3        | Terminator  |

**Output:**

| results   |
|-----------|
| Alice     |
| Titanic   |

### Explanation:

- Alice rated the highest number of distinct movies, so her name is returned.
- Titanic had the highest average rating in February 2020, so it is returned.

---

## Solution

```sql
(SELECT u.name AS results
 FROM MovieRating m
 JOIN users u ON m.user_id = u.user_id
 GROUP BY u.name
 ORDER BY COUNT(DISTINCT m.movie_id) DESC, u.name ASC
 LIMIT 1)

UNION ALL

(SELECT n.title AS results
 FROM MovieRating m1
 JOIN movies n ON n.movie_id = m1.movie_id
 WHERE MONTH(m1.created_at) = 2 AND YEAR(m1.created_at) = 2020
 GROUP BY n.title
 ORDER BY AVG(m1.rating) DESC, n.title ASC
 LIMIT 1);
```

## Solution Explanation

- The first subquery finds the user who rated the highest number of distinct movies. It joins the `MovieRating` and `users` tables, groups by the user's name, and orders by the number of distinct movies rated.
- The second subquery finds the movie that had the highest average rating in February 2020. It joins the `MovieRating` and `movies` tables, filters for ratings made in February 2020, and orders by the highest average rating.
