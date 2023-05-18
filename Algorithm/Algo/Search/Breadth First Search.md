- [[103. Binary Tree Zigzag Level Order Traversal]]
- [[102. Binary Tree Level Order Traversal]]
- [[490. The Maze]]
- [[934. Shortest Bridge]]
- [[1602. Find Nearest Right Node in Binary Tree]]

---
### Explanation:

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def bfs_queue(node):
    if node is None:
        return
    queue = []
    queue.append(node)
    while queue:
        curr_node = queue.pop(0)
        print(curr_node.value) # process current node
        if curr_node.left:
            queue.append(curr_node.left) # enqueue left child
        if curr_node.right:
            queue.append(curr_node.right) # enqueue right child

# create a binary tree
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

# perform BFS on the binary tree using queue
bfs_queue(root)
```

In this implementation, we use a queue (implemented as a Python list) to keep track of the nodes to be processed. We start by enqueueing the root node onto the queue. Then, while the queue is not empty, we dequeue the first node from the queue and process it by printing its value. We then check if the dequeued node has a left child, and if so, enqueue the left child onto the queue. Similarly, we check if the dequeued node has a right child, and if so, enqueue the right child onto the queue.

The `bfs_queue` function is similar to the previous `bfs` function, except that it uses a queue instead of recursion to perform the traversal.

In the main part of the code, we create the same binary tree as before and call `bfs_queue` with the root node to perform the breadth-first traversal. The output of the program would be:

```
1
2
3
4
5
```

alternatively, use recurions instead of queue:

```python

def bfs_recursion(queue):
    if not queue:
        return
    node = queue.pop(0)
    print(node.value) # process current node
    if node.left:
        queue.append(node.left) # enqueue left child
    if node.right:
        queue.append(node.right) # enqueue right child
    bfs_recursion(queue) # process remaining nodes in queue
    
# create a binary tree
root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)

# perform BFS on the binary tree using recursion
bfs_recursion([root])
```

---
BFS graph Search

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