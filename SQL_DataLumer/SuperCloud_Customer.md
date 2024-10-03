
# SuperCloud Customer

## Question

You are given two tables: `customer_contracts` and `products`. The `customer_contracts` table contains `customer_id` and `product_id`, and the `products` table contains `product_id` and `product_category`. Write an SQL query to find the customers who have purchased at least one product from every product category.

Return the result with the `customer_id` of the customers who have purchased from all product categories.

### Example:

**Customer Contracts Table:**

| customer_id | product_id |
|-------------|------------|
| 1           | 1001       |
| 1           | 1002       |
| 2           | 1003       |

**Products Table:**

| product_id | product_category |
|------------|------------------|
| 1001       | A                |
| 1002       | B                |
| 1003       | A                |

**Output:**

| customer_id |
|-------------|
| 1           |

### Explanation:

- Customer 1 has purchased products from both product categories (A and B).
- Customer 2 has only purchased products from category A, so they are not included.

---

## Solution

```sql
SELECT cc.customer_id
FROM customer_contracts cc 
JOIN products p  
ON cc.product_id = p.product_id
GROUP BY cc.customer_id
HAVING COUNT(DISTINCT p.product_category) = (SELECT COUNT(DISTINCT product_category) FROM products);
```

## Solution Explanation

- The query joins the `customer_contracts` and `products` tables to match customers with the categories of products they have purchased.
- It groups the result by `customer_id` and counts the distinct product categories for each customer.
- The `HAVING` clause filters for customers who have purchased products from all available categories by comparing the distinct count of product categories for each customer with the total number of distinct product categories in the `products` table.
