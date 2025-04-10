### Algorithm Code

The `GraphProcessor` class is designed for processing graph data, specifically to group nodes by their “root” node. The concept of a “root” here is derived from following edges (given as (old, new) mappings) to the terminal node, or the representative node after handling cycles. Key features include:

- **Edge Mapping Construction:**  
    Uses a `LinkedHashMap` to record the <mark style="background: #FFB8EBA6;">latest mapping from an old node to a new node</mark> while preserving the order of the <mark style="background: #FFB86CA6;">first occurrence</mark>.
    
- **Cycle Handling:**  
    The recursive helper method `findRoot` follows a node’s path using a <mark style="background: #BBFABBA6;">depth-first search (DFS) approach</mark>. If it detects that a node has already appeared in the current DFS path (indicating a cycle), it extracts the cycle and determines a <mark style="background: #FFF3A3A6;">representative node</mark>. In this implementation, the candidate for a <mark style="background: #ADCCFFA6;">cycle representative is chosen based on the node with the latest mapping update</mark>.
    
- **Overwrite Resolution:**  
    When a node appears more than once as a source (i.e., multiple mappings), only the last mapping is considered effective. Additional bookkeeping with `lastIndex` and `lastMapping` keeps track of these updates.
    
- **Grouping:**  
    After determining the root for each node, the algorithm groups all nodes under their respective root nodes.

Below is the annotated code and test cases with added explanations.

``` java
public class GraphProcessor {

    /**
     * Process graph data and group nodes by root nodes.
     * @param data A list of graph edge data, where each element is a Pair representing the mapping from the old node to the new node.
     * @return A map where the key is the root node and the value is the set of nodes belonging to that root node.
     */
    public static Map<String, Set<String>> processGraph(List<Pair<String, String>> data) {
        // Build the edge mapping (the last mapping for each key takes effect).
        // Use LinkedHashMap to preserve the insertion order of the first occurrence.
        Map<String, String> edges = new LinkedHashMap<>();
        for (Pair<String, String> pair : data) {
            edges.put(pair.old, pair.new1);
        }

        // Pre-calculate the last occurrence position (index) of each mapping and record the corresponding "new" value.
        Map<String, Integer> lastIndex = new HashMap<>();
        Map<String, String> lastMapping = new HashMap<>();
        for (int i = 0; i < data.size(); i++) {
            Pair<String, String> pair = data.get(i);
            // Always overwrite to record the latest occurrence position.
            lastIndex.put(pair.old, i);
            lastMapping.put(pair.old, pair.new1);
        }

        // Collect all nodes (including source and target nodes).
        Set<String> allNodes = new HashSet<>();
        for (Pair<String, String> pair : data) {
            allNodes.add(pair.old);
            allNodes.add(pair.new1);
        }

        // Determine the root node of each node (handle cycles).
        Map<String, String> rootMap = new HashMap<>();
        Set<String> visited = new HashSet<>();
        for (String node : allNodes) {
            if (!visited.contains(node)) {
                // If the node has not been visited, call the findRoot method to determine its root node.
                findRoot(node, edges, visited, new ArrayList<>(), rootMap, lastIndex, lastMapping);
            }
        }

        // Group nodes by root nodes.
        Map<String, Set<String>> result = new HashMap<>();
        for (String node : allNodes) {
            String root = rootMap.get(node);
            result.computeIfAbsent(root, k -> new HashSet<>()).add(node);
        }

        return result;
    }

    /**
     * Recursive method to determine the root node of each node; handle cycles.
     * @param node The node currently being processed.
     * @param edges The edge mapping between nodes.
     * @param visited The set of visited nodes.
     * @param currentPath The current depth-first search path.
     * @param rootMap The mapping from nodes to their root nodes.
     * @param lastIndex The last occurrence position of each node mapping.
     * @param lastMapping The last mapping value of each node.
     * @return The root node of the current node.
     */
    private static String findRoot(String node,
                                   Map<String, String> edges,
                                   Set<String> visited,
                                   List<String> currentPath,
                                   Map<String, String> rootMap,
                                   Map<String, Integer> lastIndex,
                                   Map<String, String> lastMapping) {
        if (rootMap.containsKey(node)) {
            return rootMap.get(node); // If the root node of the node has been determined, return it directly.
        }

        // Check if the node is already in the current DFS search path => cycle detected.
        int cycleIndex = currentPath.indexOf(node);
        if (cycleIndex != -1) {
            // Extract the cycle: all nodes from the first occurrence of the node to the end in the current path.
            List<String> cycleNodes = new ArrayList<>(currentPath.subList(cycleIndex, currentPath.size()));
            // Determine the node with the latest mapping update in the cycle.
            String candidate = null;
            int maxIdx = -1;
            for (String n : cycleNodes) {
                int idx = lastIndex.getOrDefault(n, -1);
                if (idx > maxIdx) {
                    maxIdx = idx;
                    candidate = n;
                }
            }
            // The representative node is the "new" value associated with the candidate node.
            String representative = candidate != null ? lastMapping.get(candidate) : node;
            // Mark all nodes in the cycle as having the representative node.
            for (String n : cycleNodes) {
                rootMap.put(n, representative);
                visited.add(n);
            }
            return representative;
        }

        if (visited.contains(node)) {
            return rootMap.get(node); // If the node has been visited, return its root node.
        }

        // Add the current node to the current path and mark it as visited.
        currentPath.add(node);
        visited.add(node);

        if (!edges.containsKey(node)) { // If there is no outgoing edge, the node itself is the root node.
            rootMap.put(node, node);
            currentPath.remove(currentPath.size() - 1); // Remove the current node from the current path.
            return node;
        }

        // Get the next node of the current node, recursively call the findRoot method to determine the root node of the next node, and record the root node of the current node in the rootMap.
        String next = edges.get(node);
        String root = findRoot(next, edges, visited, currentPath, rootMap, lastIndex, lastMapping);
        rootMap.put(node, root);
        currentPath.remove(currentPath.size() - 1); // Remove the current node from the current path.
        return root;
    }

    // Helper class representing an (old, new) pair.
    public static class Pair<O, N> {
        O old;
        N new1;

        public Pair(O old, N new1) {
            this.old = old;
            this.new1 = new1;
        }
    }

    // Method to conveniently create Pair objects.
    public static Pair<String, String> p(String old, String new1) {
        return new Pair<>(old, new1);
    }
}    
```

#### Explanation of Key Components

- **Edge Mapping Creation:**  
    The algorithm starts by mapping each "old" node to its corresponding "new" node using a `LinkedHashMap`. This ensures that if there are multiple edges from the same node, the last one (the effective mapping) overwrites the previous ones.
    
- **Cycle and Overwrite Support:**  
    By maintaining `lastIndex` and `lastMapping`, the algorithm captures the latest state for each node. During the DFS in `findRoot`, if a node is found in the current DFS path, a cycle exists. The cycle is resolved by choosing the node with the highest index (most recent update) and using its mapping as the representative root for the entire cycle.
    
- **DFS-based Root Discovery:**  
    The `findRoot` method recursively traverses from a node following its outgoing edge. Nodes with no outgoing edges are considered roots. As the recursion unwinds, every node’s root is recorded in `rootMap`, which facilitates the final grouping.
    
- **Grouping Nodes:**  
    Once all roots are identified, the algorithm groups nodes by their roots into a `Map<String, Set<String>>` which is finally returned.

### Test Cases
The accompanying test cases validate different scenarios:

1. **Simple Chain:**  
    Checks that nodes in a sequential edge chain (e.g., A → B → C) are grouped under the final node (root).
    
2. **Isolated Edge:**  
    Ensures that a single edge (A → B) groups A with B while treating B as its own isolated node when processed independently.
    
3. **Separate Chains:**  
    Confirms that disconnected chains (e.g., A → B and C → D) are grouped correctly under their respective roots.
    
4. **Cycle Detection:**  
    Tests that a cycle (e.g., A → B, B → C, C → A) is detected and resolved properly. In this example, the algorithm groups all nodes under the representative root (here, chosen as 'A').
    
5. **Chain Leading to Cycle:**  
    Validates that a chain that eventually enters a cycle is resolved by grouping all nodes in the chain with the cycle’s representative.
    
6. **Overwrite Edge Scenario:**  
    Verifies that when a node maps to different targets sequentially (G → H is later overwritten by G → I), only the final mapping is used.
    
7. **Inner Cycle and Jump Cycles:**  
    These more complex scenarios test nested cycles and cycles with repeated jumps between nodes. They ensure that the cycle detection logic can handle diverse patterns and still group nodes under the correct representative.

```java
public class GraphProcessorTest {  
  
    // Helper method to create a new Pair instance.  
    @Contract(value = "_, _ -> new", pure = true)  
    private GraphProcessor.@NotNull Pair<String, String> p(String oldNode, String newNode) {  
        return new GraphProcessor.Pair<>(oldNode, newNode);  
    }  
  
    @Test  
    public void testSimpleChain() {  
        // A -> B, B -> C forms a simple chain.  
        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
        data.add(p("B", "C"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected:  
        // For 'A': A -> B -> C, so the root for A, B, C becomes 'C'.        // All nodes should be grouped under key 'C'.        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("C", new HashSet<>(Arrays.asList("A", "B", "C")));  
  
        assertEquals("Simple chain grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testIsolatedEdge() {  
        // Only one edge: A -> B. Even though B is the target, it gets processed separately.  
        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected:  
        // For node A, the DFS will follow A -> B (B has no outgoing edge) and will set B as the root,        // while B is processed on its own as well. The final grouping is:        // { B: {A, B} }        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("B", new HashSet<>(Arrays.asList("A", "B")));  
  
        assertEquals("Isolated edge grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testSeparateChains() {  
        // Two disconnected chains:  
        // Chain 1: A -> B   (root will be B)        // Chain 2: C -> D   (root will be D)        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
        data.add(p("C", "D"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected groups:  
        // For chain 1: {A, B} with key 'B'        // For chain 2: {C, D} with key 'D'        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("B", new HashSet<>(Arrays.asList("A", "B")));  
        expected.put("D", new HashSet<>(Arrays.asList("C", "D")));  
  
        assertEquals("Separate chains grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testCycle() {  
        // Cycle: A -> B, B -> C, C -> A.  
        // This should detect a cycle and choose the minimum element in the cycle (i.e. 'A') as the root.        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
        data.add(p("B", "C"));  
        data.add(p("C", "A"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected:  
        // All nodes (A, B, C) should end up in the same group, with key 'A'.        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("A", new HashSet<>(Arrays.asList("A", "B", "C")));  
  
        assertEquals("Cycle grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testChainLeadingToCycle() {  
        // Test a chain that flows into a cycle:  
        // D -> E, E -> F, and F -> E forms a cycle between E and F.        // The cycle detection will yield the minimum node ('E') as the root,        // so D will also be grouped with E and F.        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("D", "E"));  
        data.add(p("E", "F"));  
        data.add(p("F", "E"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected grouping: {E: {D, E, F}}  
        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("E", new HashSet<>(Arrays.asList("D", "E", "F")));  
  
        assertEquals("Chain leading to cycle grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testOverwriteEdge() {  
        // Test that when there are multiple mappings from the same node,  
        // only the last one is used. For example, G -> H is overwritten by G -> I.        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("G", "H"));  
        data.add(p("G", "I")); // Overwrites G -> H  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        // Expected:  
        // For G, the effective edge is G -> I, so the chain G->I results in grouping {G, I} under key 'I'.        // Meanwhile, H (which was overwritten) is processed separately with no outgoing edge.        // Thus, H becomes its own group: {H}.        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("I", new HashSet<>(Arrays.asList("G", "I")));  
        expected.put("H", new HashSet<>(Arrays.asList("H")));  
  
        assertEquals("Overwrite edge grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testInnerCycle() {  
        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
        data.add(p("B", "C"));  
        data.add(p("C", "D"));  
        data.add(p("D", "B"));  
        data.add(p("B", "E"));  
        data.add(p("E", "C"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("C", new HashSet<>(Arrays.asList("A", "B", "C", "D", "E")));  
  
        assertEquals("Overwrite edge grouping failed.", expected, result);  
    }  
  
    @Test  
    public void testJumpCycles() {  
        List<GraphProcessor.Pair<String, String>> data = new ArrayList<>();  
        data.add(p("A", "B"));  
        data.add(p("B", "C"));  
        data.add(p("C", "D"));  
        data.add(p("D", "C"));  
        data.add(p("C", "D"));  
  
        Map<String, Set<String>> result = GraphProcessor.processGraph(data);  
  
        Map<String, Set<String>> expected = new HashMap<>();  
        expected.put("D", new HashSet<>(Arrays.asList("A", "B", "C", "D")));  
  
        assertEquals("Overwrite edge grouping failed.", expected, result);  
    }  
}
```