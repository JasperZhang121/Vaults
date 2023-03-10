- [[100. Same Tree]]
- [[1061. Lexicographically Smallest Equivalent String]]
- [[417. Pacific Atlantic Water Flow]]
- [[472. Concatenated Words]]
- [[1971. Find if Path Exists in Graph]]
- [[841. Keys and Rooms]]
- [[1123. Lowest Common Ancestor of Deepest Leaves]]
- [[309. Best Time to Buy and Sell Stock with Cooldown]]
- [[714. Best Time to Buy and Sell Stock with Transaction Fee]]

---

### Explanation:

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def dfs(node):
    if node is None:
        return
    print(node.value) # process current node
    dfs(node.left) # recursively process left subtree
    dfs(node.right) # recursively process right subtree

# create a binary tree
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

# perform DFS on the binary tree
dfs(root)
```

In this example, we define a `Node` class to represent a node in the binary tree. Each node has a `value` attribute to store the node's value, and `left` and `right` attributes to point to its left and right children.

The `dfs` function performs a depth-first traversal of the binary tree. It takes a `node` argument that represents the current node being processed. The function first checks if the `node` argument is `None`. If it is, then it simply returns without doing anything. Otherwise, it processes the current node by printing its value, and then recursively processes its left and right subtrees by calling `dfs` with the left and right children of the current node, respectively.

In the main part of the code, we create a binary tree with the same structure as in the previous example, and then call `dfs` with the root node of the binary tree to perform the depth-first traversal. The output of the program would be:

```
1
2
4
5
3
```

This output shows the values of the nodes in the binary tree in pre-order traversal order, which is the order in which the nodes are processed by the `dfs` function.

alternatively, use stack instead of recursion:
```python
def dfs_stack(node):
    if node is None:
        return
    stack = []
    stack.append(node)
    while stack:
        curr_node = stack.pop()
        print(curr_node.value) # process current node
        if curr_node.right:
            stack.append(curr_node.right) # push right child
        if curr_node.left:
            stack.append(curr_node.left) # push left child
```

---

### Recursion vs Stack

Recursion is often used for depth-first traversal of trees because it is a natural and intuitive way to express the traversal algorithm. When using recursion, the function essentially "calls itself" to process the children of the current node. This makes it easy to reason about the traversal and understand the flow of the algorithm.

In contrast, using a stack can be less intuitive because it requires the programmer to manually manage the stack and keep track of the nodes to be processed. This can be error-prone and harder to understand, especially for beginners who may not be familiar with the stack data structure.

That being said, using a stack has some advantages over recursion in certain cases. For example, recursion can be memory-intensive and may cause a stack overflow error if the tree is very deep. Using a stack can be more memory-efficient because it allows the programmer to control the amount of memory used by the traversal algorithm. Additionally, using a stack can make it easier to implement the traversal algorithm iteratively, which can be useful in languages that do not have tail recursion optimization or where recursive function calls are relatively expensive.
