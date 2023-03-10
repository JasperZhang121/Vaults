[[787. Cheapest Flights Within K Stops]]


---

### Explanation:

Dijkstra's algorithm is a graph traversal algorithm used to find the shortest path between a source node and all other nodes in a weighted graph. It uses a priority queue to explore the vertices in order of their distance from the source vertex. Here is an example implementation of Dijkstra's algorithm using a priority queue in Python:

```python
import heapq

def dijkstra(graph, source):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[source] = 0
    pq = [(0, source)]
    while pq:
        (current_distance, current_vertex) = heapq.heappop(pq)
        if current_distance > distances[current_vertex]:
            continue
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    return distances
```

In this implementation, we first initialize a dictionary `distances` to hold the shortest distance from the source node to all other nodes in the graph. We initialize the distance to infinity for all nodes except the source node, which has a distance of 0. We then create a priority queue `pq` to hold the vertices in order of their distance from the source vertex. The queue is initially populated with the source vertex and its distance of 0.

We then repeatedly extract the vertex with the smallest distance from the priority queue and explore its neighbors. For each neighbor, we calculate its tentative distance from the source node by adding the weight of the edge connecting it to the current node to the current distance. If this tentative distance is less than its current distance, we update the distance and add the neighbor to the priority queue.

Finally, we return the `distances` dictionary containing the shortest distances from the source node to all other nodes in the graph.

Here is an example usage of the `dijkstra` function:

```python
graph = {
    'A': {'B': 5, 'D': 9, 'E': 2},
    'B': {'C': 2},
    'C': {},
    'D': {'F': 2},
    'E': {'B': 1, 'D': 6},
    'F': {'C': 3}
}

distances = dijkstra(graph, 'A')
print(distances)
```

In this example, we define a directed graph with six nodes and eight edges, and use the `dijkstra` function to find the shortest path from the source node `'A'` to all other nodes. The output of the program would be:

```css
{'A': 0, 'B': 3, 'C': 5, 'D': 2, 'E': 2, 'F': 4}
```

This indicates that the shortest path from `'A'` to `'B'` is 3, the shortest path from `'A'` to `'C'` is 5, and so on.

---

### BFS vs Dijkstra

The main difference between BFS and Dijkstra's algorithm is that BFS is an unweighted algorithm, while Dijkstra's algorithm is a weighted algorithm. In other words, BFS treats all edges as having equal weight, whereas Dijkstra's algorithm takes into account the weight of each edge when determining the shortest path.

When exploring the graph, Dijkstra's algorithm uses a priority queue to store the vertices in order of their distance from the source vertex. This allows it to prioritize the vertices that are closest to the source and explore them first. As you mentioned, this means that whenever a new vertex is added to the queue, it is inserted based on the length of the path to that vertex from the source vertex.

Overall, Dijkstra's algorithm is a more powerful algorithm than BFS, because it can handle weighted graphs and produce accurate shortest paths. However, it also requires more computational resources than BFS, because it has to consider the weight of each edge and use a priority queue to explore the vertices in order of their distance.