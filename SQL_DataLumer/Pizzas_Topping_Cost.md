
# Pizzas Topping Cost

## Question

You are given a table `pizza_toppings` that contains `topping_name` and `ingredient_cost` for different pizza toppings. Write an SQL query to find all possible combinations of three different toppings and calculate the total cost of each combination. 

Return the result with `pizza` (a concatenated string of the three topping names in lexicographical order) and `total_cost` (the sum of the ingredient costs for the three toppings), ordered by `total_cost` in descending order and by `pizza` name in lexicographical order if there is a tie.

### Example:

**Pizza Toppings Table:**

| topping_name | ingredient_cost |
|--------------|-----------------|
| Cheese       | 2.00            |
| Pepperoni    | 3.50            |
| Mushrooms    | 1.50            |

**Output:**

| pizza                  | total_cost |
|------------------------|------------|
| Cheese, Pepperoni, Mushrooms | 7.00    |

---

## Solution

```sql
WITH cte1 AS (
    SELECT t1.topping_name AS tp1, t1.ingredient_cost AS c1, 
           t2.topping_name AS tp2, t2.ingredient_cost AS c2, 
           t3.topping_name AS tp3, t3.ingredient_cost AS c3
    FROM pizza_toppings t1
    JOIN pizza_toppings t2 ON t1.topping_name < t2.topping_name
    JOIN pizza_toppings t3 ON t2.topping_name < t3.topping_name
)

SELECT pizza, total_cost
FROM (
    SELECT CONCAT(tp1, ',', tp2, ',', tp3) AS pizza, c1 + c2 + c3 AS total_cost
    FROM cte1
) x 
ORDER BY total_cost DESC, pizza;
```

## Solution Explanation

- The query first generates all possible combinations of three different toppings using three self-joins on the `pizza_toppings` table, ensuring that each topping name is lexicographically ordered.
- The main query calculates the total cost of the three toppings and concatenates the topping names into a string.
- The result is ordered by `total_cost` in descending order and by `pizza` name in lexicographical order if there is a tie.
