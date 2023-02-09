## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/find-eventual-safe-states/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search, Breadth-First Search, Topological Sort
* 🗒️ **Similar Questions**: [Build a Matrix With Conditions](https://leetcode.com/problems/build-a-matrix-with-conditions/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What do we need to do with the nodes?
  - This question is about to find the nodes who is not in a cycle, which means itself and its children are not in a cycle.
- What are 4 states we can use to record a node's status?
  - We take 4 states to record a node's status: {UNKNOW, VISIT, SAFE, UNSAFE}. UNKNOWN: current node is not visited. Initialized status. VISIT: current node is in the process of dfs. SAFE: current node is safe. All childs are SAFE -> current is SAFE. UNSAFE: current node is unsafe.
- What do we do to nodes that are eventually safe?  
  - Return them as an array in sorted order.
    
```markdown
HAPPY CASE
Input: graph = [[1,2],[2,3],[5],[0],[5],[],[]]
Output: [2,4,5,6]
Explanation: The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.
  

EDGE CASE
Input: graph = [[1,2,3,4],[1,2],[3,4],[0,4],[]]
Output: [4]
Explanation:
Only node 4 is a terminal node, and every path starting at node 4 leads to node 4.
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- BFS: We can utilize BFS and Djikstra’s algorithm. This problem is to find shortest path, the only thing we need to pay attention to is the weight of edge from `grid[0][0]` to `grid[n-1][n-1]`. We can take grid `[[0, 1], [2, 3]]` as an example: We can treat this as a graph with 4 vertices `0,1,2,3` and there are 4 edges. `{0, 1}` with weight 1, `{0, 2}` with weight `2`, `{1, 3}` with weight `3` and `{2, 3}` with weight `3` respectively. Each time we choose the smallest edge `{m, n}` to simulate at time `max(m, n)` and rain falls. So we first take edge `{0, 1}` then `{0, 2}`. When we are processing edge `{1, 3}`, the original result is `2(getting from {0, 2})`, but since original time is less than we currently need, we need time 3 to reach vertex 3. For another case like grid `[[3, 2], [1, 0]]`, the first part is the same. To reach from vertex 3 to vertex 1, we need time 3. Then to reach from vertex 1 to vertex 0, we also only need time 3, since we can move infinite distance in one move. We can see this like at time 3, water can move from vertex 3 to vertex 1 then vertex 0 directly. From 2 cases above, we can get the most important thing in this problem that the time needs to reach `grid[i][j]` is `max(grid[i][j], grid[i'][j'])`, where `grid[i'][j']` is the cell to get to `grid[i][j]`.
- DFS: Another option is to a DFS. How do we know that a path exists? Here, we will travel in all the four directions (up, down, left, right) by not visiting the node we have visited earlier. And in this traversal we start from `[0,0]` and if we happen to touch `[n-1,m-1]`, then we can say that a path exists. This is general Depth First Traversal, but we have another constraint that we cannot move to a `height > t`, so we include a constraint that we can move in any of the four directions, if and only if, the height of the building in that direction is less than `t`.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Map: We can use a map to store the edges in the graph to lookup equations by name, and store all neighbors by name.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** In a directed graph, we start at some node and every turn, walk along a directed edge of the graph.  If we reach a node that is terminal (that is, it has no outgoing directed edges), we stop. Now, say our starting node is eventually safe if and only if we must eventually walk to a terminal node.  More specifically, there exists a natural number K so that for any choice of where to walk, we must have stopped at a terminal node in less than K steps.
The directed graph has N nodes with labels 0, 1, ..., N-1, where N is the length of graph.  The graph is given in the following form: graph[i] is a list of labels j such that (i, j) is a directed edge of the graph. NOTE that we may do some pruning optimization.
  
To know if the path is not safe, we check if it goes back to the ongoing path. Therefore, I have a set visiting to store nodes that are in the current DFS. I make sure to remove the node once DFS finishes.
    
```markdown
1. When we visit a node, the only possibilities are that we’ve marked the entire subtree black (which must be eventually safe), or it has a cycle and we have only marked the members of that cycle gray. So the invariant that gray nodes are always part of a cycle, and black nodes are always eventually safe is maintained.

2. In order to exit our search quickly when we find a cycle (and not paint other nodes erroneously), we’ll say the result of visiting a node is true if it is eventually safe, otherwise false. This allows information that we’ve reached a cycle to propagate up the call stack so that we can terminate our search early.
```

⚠️ **Common Mistakes**

* The crux of the problem is whether you can reach a cycle from the node you start in. If you can, then there is a way to avoid stopping indefinitely; and if you can't, then after some finite number of steps you'll stop. Thinking about this property more, a node is eventually safe if all it's outgoing edges are to nodes that are eventually safe. 
  
We start with nodes that have no outgoing edges - those are eventually safe. Now, we can update any nodes which only point to eventually safe nodes - those are also eventually safe. Then, we can update again, and so on.
However, we'll need a good algorithm to make sure our updates are efficient.
    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```python
class Solution:
    def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
        
        dt = defaultdict(list)
        status = defaultdict(int) # if visiting this node change status to 1 else 0
        res = []
        
        for i, val in enumerate(graph):
            dt[i] = val
            
        def dfs(num):
            if dt[num] == []: return True
            
            if status[num] == 1: return False
            
            status[num] = 1
            
            for val in dt[num]:
                if not dfs(val):
                    return False
                
            dt[num] = []
            status[num] = 0
            
            return True
            
        for num in range(len(graph)):
            if dfs(num):
                res.append(num)

        return res
```

```java
class Solution {
    public List<Integer> eventualSafeNodes(int[][] graph) {
        List<Integer> res = new ArrayList<>();
        int n = graph.length;  //number of nodes
        int[] visited = new int[n];  //3 status, 0: unvisited, 1: visiting, 2: visited
        
        for (int i = 0; i < n; i++)  // try all nodes
            if (DFS(graph, i, visited))
                res.add(i);
        return res;
    }
    
    //return true if node i is safe
    private boolean DFS(int[][] graph, int i, int[] visited) {
        if (visited[i] == 1)  //node i is being visited, so cycle
            return false;
        
        visited[i] = 1;  //visiting
        for (int neighbor : graph[i]) {
            int status = visited[neighbor];  //status of neighbor
            if (status == 1)   //neighbor is a node we are visiting, so cycle. All node in a cycle is not safe
                return false;
            if (status == 2)   //already visited
                continue;
            //otherwise, status == 0, meaning ok to visit
            if (!DFS(graph, neighbor, visited)) 
                return false;
        }
        visited[i] = 2;  //visited
        return true;
    }
}
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity - O(V + E)
<br>
Space Complexity - O(V + E)