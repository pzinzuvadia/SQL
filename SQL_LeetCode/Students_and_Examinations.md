
# LeetCode Problem: Students and Examinations

## Problem Statement

[**Students and Examinations**](https://leetcode.com/problems/students-and-examinations/submissions/1388344824/?envType=study-plan-v2&envId=top-sql-50)

**Description:**

You are given three tables: `Students`, `Subjects`, and `Examinations`. Write a SQL query to find the number of examinations each student attended for every subject.

### Table: `Students`

| Column Name  | Type    |
|--------------|---------|
| student_id   | int     |
| student_name | varchar |

`student_id` is the primary key for this table. This table contains information about students.

### Table: `Subjects`

| Column Name  | Type    |
|--------------|---------|
| subject_name | varchar |

`subject_name` is the primary key for this table. This table contains information about subjects.

### Table: `Examinations`

| Column Name  | Type    |
|--------------|---------|
| student_id   | int     |
| subject_name | varchar |

There is no primary key for this table. This table contains information about student attendance for examinations in various subjects. Each row records that a student attended an exam for a subject.

### Task:
For each student, return the number of exams they attended for every subject.

## Sample Input:

### Students Table:

| student_id | student_name |
|------------|--------------|
| 1          | Alice        |
| 2          | Bob          |

### Subjects Table:

| subject_name |
|--------------|
| Math         |
| Physics      |

### Examinations Table:

| student_id | subject_name |
|------------|--------------|
| 1          | Math         |
| 1          | Physics      |
| 1          | Math         |
| 2          | Math         |

## Expected Output:

| student_id | student_name | subject_name | attended_exams |
|------------|--------------|--------------|----------------|
| 1          | Alice        | Math         | 2              |
| 1          | Alice        | Physics      | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |

### Explanation:
- Alice attended 2 Math exams and 1 Physics exam.
- Bob attended 1 Math exam but did not attend any Physics exam.

## SQL Solution:

```sql
SELECT s.student_id, s.student_name, sub.subject_name, COUNT(e.student_id) AS attended_exams
FROM students s
JOIN subjects sub
LEFT JOIN examinations e
ON e.student_id = s.student_id AND sub.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```

## Solution Explanation:

1. **LEFT JOIN**: We perform a `LEFT JOIN` between the `Examinations` table and the combination of `Students` and `Subjects` to include all students and subjects, even if there are no matching rows in the `Examinations` table.

2. **ON Condition**: The join condition ensures we match students and subjects across the tables based on `student_id` and `subject_name`.

3. **COUNT(e.student_id)**: We count the number of times each student attended an exam for each subject. If a student did not attend any exams for a subject, the count will be `0`.

4. **GROUP BY**: We group the results by `student_id`, `student_name`, and `subject_name` to compute the number of exams attended per student for each subject.

5. **ORDER BY**: The results are ordered by `student_id` and `subject_name` for better readability.

## Conclusion:

This query provides a breakdown of how many exams each student attended for every subject, including subjects they did not attend any exams for.
