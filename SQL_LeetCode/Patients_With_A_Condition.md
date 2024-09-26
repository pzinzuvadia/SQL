
# Patients With a Condition

## Question

Write an SQL query to find the patients who have been diagnosed with the condition "DIAB1" (Diabetes Type 1). The condition may appear at the beginning or in the middle of the `conditions` field.

Return the result table with the `patient_id`, `patient_name`, and `conditions`.

### Example:

**Patients Table:**

| patient_id | patient_name | conditions     |
|------------|--------------|----------------|
| 1          | John Doe     | DIAB1, HYPERT  |
| 2          | Jane Smith   | HYPERT         |
| 3          | Mike Johnson | HYPERT, DIAB1  |

**Output:**

| patient_id | patient_name | conditions     |
|------------|--------------|----------------|
| 1          | John Doe     | DIAB1, HYPERT  |
| 3          | Mike Johnson | HYPERT, DIAB1  |

### Explanation:

- Both John Doe and Mike Johnson have "DIAB1" listed as one of their conditions.

---

## Solution

```sql
SELECT patient_id, patient_name, conditions
FROM patients
WHERE conditions LIKE 'DIAB1%' OR conditions LIKE '% DIAB1%';
```

## Solution Explanation

- The `LIKE` operator is used to search for the condition "DIAB1" in the `conditions` field.
- The first `LIKE` condition checks if "DIAB1" appears at the beginning of the field, and the second `LIKE` condition checks if it appears after other conditions.
