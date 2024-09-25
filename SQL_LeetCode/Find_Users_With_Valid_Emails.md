
# Find Users With Valid E-Mails

## Question

Write an SQL query to find the users who have a valid email address. A valid email address satisfies the following criteria:

1. The email starts with a letter.
2. The email contains only letters, numbers, dashes ('-'), underscores ('_'), or periods ('.') before the '@' symbol.
3. The email ends with '@leetcode.com'.

Return the `user_id`, `name`, and `mail` of the users with valid email addresses.

### Example:

**Users Table:**

| user_id | name   | mail                  |
|---------|--------|-----------------------|
| 1       | Alice  | alice@leetcode.com    |
| 2       | Bob    | bob@leet_code.com     |
| 3       | Charlie| charlie_01@leetcode.com |

**Output:**

| user_id | name   | mail                  |
|---------|--------|-----------------------|
| 1       | Alice  | alice@leetcode.com    |
| 3       | Charlie| charlie_01@leetcode.com |

### Explanation:

- Alice and Charlie have valid emails, but Bob's email is invalid because it contains an underscore after the '@' symbol.

---

## Solution

```sql
SELECT user_id, name, mail
FROM users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9\-\._]*@leetcode\.com$';
```

## Solution Explanation

- The `REGEXP` function is used to match email addresses based on the provided regular expression pattern.
- The pattern checks that the email starts with a letter, followed by valid characters (letters, numbers, dashes, periods, or underscores) before the '@leetcode.com' domain.
