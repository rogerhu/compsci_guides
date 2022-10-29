## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/cheapest-flights-within-k-stops/](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search, Breadth-First Search, Dijkstra’s
* 🗒️ **Similar Questions**: [Maximum Vacation Days](https://leetcode.com/problems/maximum-vacation-days/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- When does a given path given priority over another?
  - In a basic Dijkstra algorithm, the only basis upon which one path is given priority over another is the cost. In this problem however, there is an additional constraint; length of route or (number of stops).
    
- What do we need to keep track of during the traversal?
  - We need to keep track of all routes to the node and compare on the basis of (cost and number of stops).


```markdown
    HAPPY CASE
    Input: n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
    Output: 700
    
    Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
    Output: 200
    
    EDGE CASE
    Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
    Output: 500
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- Dijkstra's: We can use Dijkstra’s algorithm to find minimum path from single source with edge weights. The main trick to this problem is we have two constraints to consider in our Dijkstra's implementation. The priority queue must sort by stops and then by cost. We must sort by stops before cost. The issue arises when we find a less cost, but greater stop number path to an intermediary node. From `node0` -> `node1`, we can take `path 0->1`, with 5 cost and 1 stop, or we could take `0->3->1`, with 4 cost and 2 stops. If we update our Dijkstra's shortest path cache to be 4 cost, then we will miss out on a better cost because k = 2 already by taking the shortest path to 1. By taking the shortest cost path to 1 (0->3->1), we already used our 2 stops, so our next node must be our goal of `node2` to not violate `k` constraint. The actual shortest path is `0->1->4->2` where we take a higher cost path to 1, `0->1` cost 5, but `k = 1`.
- Adjacency List: We can use an adjacency list to store graph, but equation variables cannot be directly translated to an index.
- Adjacency Matrix: We can use an adjacency matrix to store graph, but will cause runtime slowdowns of O(N^2) for a sparse graph and run into similar issues as with adjacency list.
- Map: We can use a map to store the edges in the graph to lookup equations by name, and store all neighbors by name.
- Topological Sort: We can use topological sort to traverse the graph, similar to BFS and DFS, and find the result.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

    To implement Dijkstra, we need a priority queue to pop out the lowest weight node for next search. In this case, the weight would be the accumulated flight cost. So my node takes a form of `(cost, src, k)`. `cost` is the accumulated cost, `src` is the current node's location, `k` is stop times we left as we only have at most K stops. I also convert `edges` to an adjacent list based graph `g`.
    
    Use a `vis` array to maintain visited nodes to avoid loop. `vis[x]` record the remaining steps to reach x with the lowest cost. If `vis[x] >= k`, then no need to visit that case `(start from x with k steps left)` as a better solution has been visited before (more remaining step and lower cost as heappopped beforehand). And we should initialize `vis[x]` to `0` to ensure visit always stop at a negative `k`.
    
    Once `k` is used up (`k == 0`) or `vis[x] >= k`, we no longer push that node `x` to our queue. Once a popped cost is our destination, we get our lowest valid cost.
    
    For Dijkstra, there is not need to maintain a `best cost` for each node since it's kind of greedy search. It always chooses the lowest cost node for next search. So the previous searched node always has a lower cost and has no chance to be updated. The first time we pop our destination from our queue, we have found the lowest cost to our destination.


⚠️ **Common Mistakes**

* 
    
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
    class Solution {
      int UNVISITED = 10000000;
      
      public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        Map<Integer, List<int[]>> graph = buildGraph(flights, n);
        
        int result = dijkstra(graph, src, dst, k, n);
        return result;
      }
      
      public int dijkstra(
        Map<Integer, List<int[]>> graph,
        int start, int dest, int k, int n
      ) {
        int[] dist = new int[n];
        Arrays.fill(dist, UNVISITED);
        dist[start] = 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> {
          if(a[2] == b[2]) return a[1] - b[1]; 
          return a[2] - b[2]; 
        });
        pq.offer(new int[]{start, 0, 0});
        
        while(!pq.isEmpty()) {
          int[] item = pq.poll();
          int node = item[0]; 
          int cost = item[1]; 
          int stops = item[2]; 
          
          if(stops > k) continue;
          
          List<int[]> neighbors = graph.get(node);
          
          for(int[] neighbor : neighbors) {
            int neighborNode = neighbor[0];
            int neighborCost = neighbor[1];
            int totalCost = cost + neighborCost;
            
            if(totalCost < dist[neighborNode]) {
              dist[neighborNode] = totalCost;
              pq.offer(new int[]{neighborNode, totalCost, stops+1});
            }
          }
        }
        
        return dist[dest] == UNVISITED ? -1 : dist[dest];
      }
      
      public Map<Integer, List<int[]>> buildGraph(int[][] flights, int n) {
        Map<Integer, List<int[]>> graph = new HashMap<>();
        
        for(int i = 0; i < n; i++) graph.put(i, new ArrayList<>());
        
        for(int[] flight : flights) {
          int src = flight[0];
          int dest = flight[1];
          int cost = flight[2];
          
          graph.get(src).add(new int[]{dest, cost});
        }
        
        return graph;
      }
    }
```
    
```python
    def findCheapestPrice(n, flights, src, dst, K):
    	graph = collections.defaultdict(dict)
    	for s, d, w in flights:
    		graph[s][d] = w
    	pq = [(0, src, K+1)]
    	vis = [0] * n
    	while pq:
    		w, x, k = heapq.heappop(pq)
    		if x == dst:
    			return w
    		if vis[x] >= k:
    			continue
    		vis[x] = k
    		for y, dw in graph[x].items():
    			heapq.heappush(pq, (w+dw, y, k-1))
    	return -1
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(KE)`, where *E* is # of edges and K is the # of stops
<br>
Space Complexity: `O(V)`, where *V* is the # of nodes in graph