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

---
DFS graph

```python
def breadth_first_search(graph, start_node="A", end_node="D"):
    """
        Perform a breadth-first search on a graph to find a target node.

        Parameters:
        -----------
        graph : dict
            A dictionary representing the graph, with node names as keys and lists of neighbor nodes as values.
        start_node : str, optional
            The node to start the search from. Default is "A".
        end_node : str, optional
            The node to search for. Default is "D".

        Returns:
        --------
        bool
            True if the target node is found, False otherwise.

        Prints:
        -------
        str
            If the target node is found, prints the path taken to reach it, along with the depth of the node.

        Examples:
        ---------
        >>> graph = {'A': ['B', 'C'], 'B': ['D'], 'C': ['D'], 'D': []}
        >>> breadth_first_search(graph)
        Found D at depth 2, the path taken is: A->C->D
        True
        """

    frontier, discovered, depth = [(start_node, [start_node])], {start_node}, 0

    while frontier:
        current, move = frontier.pop(0)
        if current == end_node:
            path = "->".join(move)
            print(f"BFS: found {end_node} by depth {depth}, the path taken is: {path}")
            return True
        for neighbor in graph[current]:
            if neighbor not in discovered:
                temp = move.copy()
                temp.append(neighbor)
                frontier.append((neighbor, temp))
                discovered.add(neighbor)
        depth += 1

    print("Cannot found the target")
    return False
```