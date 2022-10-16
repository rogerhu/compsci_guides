🔗 **Leetcode Link:** [https://leetcode.com/problems/evaluate-division](https://leetcode.com/problems/evaluate-division)

⏰ **Time to complete**: __ mins

1. **U-nderstand**
    - How many equations can we have?
        - “We can have up to 20 questions.”
    - Can there be no questions?
        - “No, there must be a minimum of one question.”
    - How long can variable names be?
        - “There can be up to 5 characters in a variable name.”
    - Can variable names be empty?
        - “No, variable names must have at least one alphabetical character”
    - How many queries are performed?
        - “There can be up to 20 queries.”
    
    ```markdown
    HAPPY CASE
    Input:
        equations = [["a","b"],["b","c"],["bc","cd"]]
        values = [1.5,2.5,5.0]
        queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
    
    Output: [3.75000,0.40000,5.00000,0.20000]
    
    EDGE CASE
    Input:
        equations = [["a","b"],["b","c"]]
        values = [2.0,3.0]
        queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
    
    Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
    ```
    
2. M-atch
    
    For graph problems, some things we want to consider are:
    
    - We can use BFS to traverse the equation graph, and accumulate the products of the edges to find the answer.
    - We can use DFS to traverse the equation graph, and accumulate the products of the edges to find the answer, while ignoring invalid paths.
    - We can use an adjacency list to store graph, but equation variables cannot be directly translated to a index.
    - We can use an adjacency matrix to store graph, but will cause runtime slowdowns of O(N^2) for a sparse graph and run into similar issues as with adjacency list.
    - We can use a map to store the edges in the graph to lookup equations by name, and store all neighbors by name.
    - We can use topological sort to traverse the graph, similar to BFS and DFS, and find the result.
    - **BONUS** – We can use floyd warshall to perform all node minimum distance computation, allowing O(1) query.
3. P-lan
    
    We will build a graph of the equation where the nodes are variables, edges are the equations, and edge weights are the equation values. Then, we will perform a topological sort to return the weight contribution of the children to the parent node.
    
    `1) Create a map as our graph. 2) For each equation, i
      a) Add edge from A to B with V[i]
      b) Add edge from B to A with 1/V[i]
    3) For each query
        a) Perform topological sort from start node
            i)   Keep track of product weights for each neighbor, 
                computed by neighbor * topoSort(neighbor)
            ii)  If all neighbor product weights were -1, return -1
            iii) Return tracked product weights
        b) Add result of topological sort call from start to end variable to list
    4) Return result list
    
    Time Complexity: O(N*K)
    Space Complexity: O(N)`
    
    **Common Mistakes:**
    
    - Make sure you really clarify all inputs. When treating this problem as a graph problem, they may assume information that can significantly affect runtime complexity. For instance, assuming the lengths of the variable names or number of equations greatly affects which algorithms can be used and how they are used. In this case, students should cover most, if not all, clarifications to develop an efficient algorithm.
4. I-mplement
    
    ```java
    // Java Code
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    
        /* Build graph. */
        Map<String, Map<String, Double>> graph = buildGraph(equations, values);
        double[] result = new double[queries.size()];
        
        for (int i = 0; i < queries.size(); i++) {
            result[i] = getPathWeight(queries.get(i).get(0), queries.get(i).get(1), new HashSet<>(), graph);
        }  
        
        return result;
    }
    private double getPathWeight(String start, String end, Set<String> visited, Map<String, Map<String, Double>> graph) {
        
        /* Rejection case. */
        if (!graph.containsKey(start)) 
            return -1.0;
        
        /* Accepting case. */
        if (graph.get(start).containsKey(end))
            return graph.get(start).get(end);
        
        visited.add(start);
        for (Map.Entry<String, Double> neighbour : graph.get(start).entrySet()) {
            if (!visited.contains(neighbour.getKey())) {
                double productWeight = getPathWeight(neighbour.getKey(), end, visited, graph);
                if (productWeight != -1.0)
                    return neighbour.getValue() * productWeight;
            }
        }
        
        return -1.0;
    }
    private Map<String, Map<String, Double>> buildGraph(List<List<String>> equations, double[] values) {
        Map<String, Map<String, Double>> graph = new HashMap<>();
        String u, v;
        
        for (int i = 0; i < equations.size(); i++) {
            u = equations.get(i).get(0);
            v = equations.get(i).get(1);
            graph.putIfAbsent(u, new HashMap<>());
            graph.get(u).put(v, values[i]);
            graph.putIfAbsent(v, new HashMap<>());
            graph.get(v).put(u, 1 / values[i]);
        }
    ```
    
    ```python
    # Python Code
    def calcEquation(equations, values, queries):
      graph = buildGraph(equations, values)
      results = []
      for query in queries:
        result = getPathWeight(query[0], query[1], graph, set())
        results.append(result)
    
      return results
    
    def getPathWeight(start, end, graph, visited):
      if start not in graph:
        return -1
    
      if end in graph[start]:
        return graph[start][end]
    
      visited.add(start)
    
      for neighbor in graph[start]:
        if neighbor not in visited:
          productWeight = getPathWeight(neighbor, end, graph, visited)
          if productWeight != -1:
                return graph[start][neighbor] * productWeight
            
      return -1
    
    def buildGraph(equations, values):
      graph = {}
    
      for equation, value in zip(equations, values):
        u, v = equation[0], equation[1]
        if u not in graph: graph[u] = {}
        graph[u][v] = value
        if v not in graph: graph[v] = {}
        graph[v][u] = 1 / value
    
      return graph
    ```
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: O(N*K)
    - Space Complexity: O(N)