🔗 **Leetcode Link:** [https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)

⏰ **Time to complete**: 20 mins

1. **U-nderstand**

- Do we need on the node or the keys in the graph?
We are told that we are guaranteed to have all nodes from 0 -> n. That's why we need to loop on the range (0 -> n) instead of the keys in your graph representation (or the edge list) because nodes that are completely isolated need to be treated as their own connected component.
    
- Are the BFS and DFS solutions same here?
DFS and BFS are similar. First we build an adjacent-list graph base on edges. Then we iterate all nodes in graph: `{0,...,n-1}`. In each iteration, we can either DFS or BFS to remove all connected component nodes. The times that node is still in our graph when we iterate it is the number of connected components.
    
    ```markdown
    HAPPY CASE
    Input: n = 5, edges = [[0,1],[1,2],[3,4]]
    Output: 2
    
    Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
    Output: 1
    ```
    
2. M-atch
    
    For graph problems, some things we want to consider are:
    
    - BFS - The idea is to have an unvisited set from 0 to `n-1`. We have the edge map, representing edges. For every non-visited node, we add it to the BFS queue. We run the BFS. If there's remaining nodes, we add it to the BFS queue again incrementing the count, since this is an unconnected component. We repeat until all nodes are visited.
    
    The runtime is O(E+V) where V = number of nodes and V = number of edges in the entire graph (all connected components) because you only drill down on a node (and all its neighbors) if you haven't seen it before.
    
    - Union Find: Basically, we want to minimize the height of the tree to reduce the number of operations of finding the parent node. In order to prevent generating a skewed tree, we should apply the weighted technique. The weighted technique records the number of nodes of a set in the corresponding root node as a negative number as shown in the code. Whenever two sets are about to be unioned, we calculate the total number of nodes and set one of the root with the larger number as the new root of the newly union set.
3. P-lan
    
    General Description of plan (1-2 sentences)
    
    - Build the undirected graph.
    - Loop over the nodes and run a BFS on the node if it has not been explored before. It will behave as a sink that will swallow each connected component allowing you to increment a counter.
    - To make your algorithm more efficient, use a global visited set for the entire graph rather than a new visited set for each component.
4. I-mplement
    
    ```java
    class Solution {
        ArrayList<ArrayList<Integer>> adj = new ArrayList();
        public int countComponents(int n, int[][] edges) {
            boolean [] visited = new boolean[n];
            for(int i=0;i<n;i++){
                adj.add(new ArrayList());
            }
            for(int [] edge:edges){
                int u =edge[0];
                int v = edge[1];
                adj.get(u).add(v);
                adj.get(v).add(u);
            }
            int count=0;
            for(int i=0;i<n;i++){
                if(visited[i]==false){
                  dfs(adj,visited,i);
                count++;
                }
               
                
            }
            return count;
        }
        
        private void dfs(ArrayList<ArrayList<Integer>> adj, boolean[] visited,int s){
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
            adj_list = [[] for _ in range(n)]
            for n1, n2 in edges:
                adj_list[n1].append(n2)
                adj_list[n2].append(n1)
    
            count = 0
            seen = set()
            queue = collections.deque()
            
            for i in range(n):
                if i not in seen:
                    queue.append(i)
                    seen.add(i)
                    count += 1
                    self.bfs(adj_list, queue, seen)
            return count
    
        def bfs(self, adj_list, queue, seen):
            while queue:
                node = queue.popleft()
                for neighbour in adj_list[node]:
                    if neighbour not in seen:
                        seen.add(neighbour)
                        queue.append(neighbour)
    ```
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: `O(E + V)`, where `E` = Number of edges, `V` = Number of vertices
    - Space Complexity: `O(E + V)`, where `E` = Number of edges, `V` = Number of vertices