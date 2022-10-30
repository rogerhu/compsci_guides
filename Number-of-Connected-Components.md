## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: [Number of Islands](https://leetcode.com/problems/number-of-islands/), [Paths in Maze That Lead to Same Room](https://leetcode.com/problems/paths-in-maze-that-lead-to-same-room/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Do we need on the node or the keys in the graph?
  - We are told that we are guaranteed to have all nodes from 0 -> n. That's why we need to loop on the range (0 -> n) instead of the keys in your graph representation (or the edge list) because nodes that are completely isolated need to be treated as their own connected component.
    
- Are the BFS and DFS solutions same here?
  - DFS and BFS are similar. First we build an adjacent-list graph base on edges. Then we iterate all nodes in graph: `{0,...,n-1}`. In each iteration, we can either DFS or BFS to remove all connected component nodes. The times that node is still in our graph when we iterate it is the number of connected components.
    
```markdown
    HAPPY CASE
    Input: n = 5, edges = [[0,1],[1,2],[3,4]]
    Output: 2
    
    Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
    Output: 1
```
    
## 2: M-atch
    
> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

    For graph problems, some things we want to consider are:
    
- BFS: The idea is to have an unvisited set from 0 to `n-1`. We have the edge map, representing edges. For every non-visited node, we add it to the BFS queue. We run the BFS. If there's remaining nodes, we add it to the BFS queue again incrementing the count, since this is an unconnected component. We repeat until all nodes are visited. The runtime is O(E+V) where V = number of nodes and V = number of edges in the entire graph (all connected components) because you only drill down on a node (and all its neighbors) if you haven't seen it before.
- Union Find: Basically, we want to minimize the height of the tree to reduce the number of operations of finding the parent node. In order to prevent generating a skewed tree, we should apply the weighted technique. The weighted technique records the number of nodes of a set in the corresponding root node as a negative number as shown in the code. Whenever two sets are about to be unioned, we calculate the total number of nodes and set one of the root with the larger number as the new root of the newly union set.
- DFS: We can use DFS to traverse the graph until we find a valid itinerary, ensuring we choose the lexicographically least itinerary.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Run a DFS, starting from a particular vertex, it will continue to visit the vertices depth-wise until there are no more adjacent vertices left to visit. 
    
1. Build the undirected graph.
2. Loop over the nodes and run a BFS on the node if it has not been explored before. It will behave as a sink that will swallow each connected component allowing you to increment a counter.
3. To make your algorithm more efficient, use a global visited set for the entire graph rather than a new visited set for each component.

⚠️ **Common Mistakes**

* We may apply the union find algorithm where it uses Union by rank. The time complexity for this is O(log(N)) for each merge. We can improve this with both union by rank and path compression, which will offer us ~O(1) time for each union/find operation, so it gives O(V + E) time in total.

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        ArrayList<ArrayList<Integer>> adj = new ArrayList();
        public int countComponents(int n, int[][] edges) {
            boolean [] visited = new boolean[n];
            for(int i = 0; i < n; i++){
                adj.add(new ArrayList());
            }
            for(int [] edge:edges){
                int u =edge[0];
                int v = edge[1];
                adj.get(u).add(v);
                adj.get(v).add(u);
            }
            int count=0;
            for(int i = 0;i < n;i++){
                if(visited[i] == false) {
                  // apply dfs
                  dfs(adj, visited, i);
                count++;
                }
            }
            return count;
        }
        
        // use dfs to traverse the graph to see if all the nodes can be covered
        private void dfs(ArrayList<ArrayList<Integer>> adj, boolean[] visited, int s){
            visited[s]=true;
            for(int v:adj.get(s)){
                if(visited[v]==false){
                    dfs(adj,visited,v);
                }
            }
        }
    }
```
    
```python
    class Solution:
        def countComponents(self, n: int, edges: List[List[int]]) -> int:
            # create adjacency list
            adj_list = [[] for _ in range(n)]
            for n1, n2 in edges:
                adj_list[n1].append(n2)
                adj_list[n2].append(n1)
    
            count = 0

            # keep track of visited nodes in set
            seen = set()
            queue = collections.deque()
            
            for i in range(n):
                if i not in seen:
                    queue.append(i)
                    seen.add(i)
                    count += 1
                    self.bfs(adj_list, queue, seen)
            return count
    
        # apply bfs
        def bfs(self, adj_list, queue, seen):
            while queue:
                node = queue.popleft()
                for neighbour in adj_list[node]:
                    if neighbour not in seen:
                        seen.add(neighbour)
                        queue.append(neighbour)
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(E + V)`, where `E` = Number of edges, `V` = Number of vertices
<br>
Space Complexity: `O(E + V)`, where `E` = Number of edges, `V` = Number of vertices