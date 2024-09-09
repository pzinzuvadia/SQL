
# Binary Search Tree Nodes
[**HackerRank Link**](https://www.hackerrank.com/challenges/binary-search-tree-1?isFullScreen=true)

---

## Question Explanation:
The problem asks us to classify each node of a binary search tree into one of three categories:
1. **Root** - The node with no parent.
2. **Leaf** - A node that is not a parent of any other node.
3. **Inner** - A node that is neither a root nor a leaf.

Given the structure of the tree, we are required to determine the position of each node as either Root, Leaf, or Inner.

---

## Sample Tables:
The table `bst` contains two columns:
- `n` - Node identifier
- `p` - Parent node identifier (null for the root)

Example table:

| n  | p  |
|----|----|
| 1  | 2  |
| 2  | 4  |
| 3  | 2  |
| 4  | null |

---

## Answer:
Here’s the SQL query that solves the problem:

```sql
select n, case 
    when p is null then 'Root'
    when n not in (select p from bst where p is not null) then 'Leaf'
    else 'Inner' 
end as position
from bst
order by n;
```

---

## Answer Explanation:
1. The **Root** is identified by the condition `p is null`, meaning the node has no parent.
2. **Leaf** nodes are those where the node identifier `n` does not appear as a parent `p` in the tree (`n not in (select p from bst where p is not null)`).
3. **Inner** nodes are those that are neither the root nor leaf, identified by the `else 'Inner'` condition.

The query orders the output by node `n` to ensure the nodes are presented in increasing order.
