# Wiki Solutions Guide

## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/connecting-cities-with-minimum-cost/>
* **Difficulty:** Medium
* **Time to complete**: __ mins
* **Topics**: Graphs, Dijkstra's, Union Find, Minimum Spanning Tree
* **Similar Questions**: [Minimum Cost to Reach City With Discounts](https://leetcode.com/problems/minimum-cost-to-reach-city-with-discounts/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How can we find the minimum weight path?
  - You can use Dijkstra's algorithm to find the minimum weight path. Keep track of the minimum distance to each vertex with d discounts left
- Are there duplicate highways?
  - There are no duplicate highways.
   
```markdown
HAPPY CASE
Input: n = 5, highways = [[0,1,4],[2,1,3],[1,4,11],[3,2,3],[3,4,2]], discounts = 1
Output: 9

Input: n = 4, highways = [[1,3,17],[1,2,7],[3,2,5],[0,1,6],[3,0,20]], discounts = 20
Output: 8

EDGE CASE
Input: n = 4, highways = [[0,1,3],[2,3,2]], discounts = 0
Output: -1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graph problems, we want to consider the following approaches:

* Dijkstra’s: We essentially need to find the minimum distance to get from node 0 to node n - 1 in an undirected weighted graph. What algorithm should we use to do this? Since, Dijkstra’s algorithm that seeks the minimum weighted vertex on every iteration, so the original Dijkstra’s algorithm will output the first path but the result should be the second path as it contains minimum number of edges.
* Prim's: With Prim's, it constructs an MST from the point of view. The general idea is: Let the set of vertices in the graph G be U, first Arbitrarily choose a point in the graph G as the starting point a, add this point to the set V, and then find another point b from the set UV to make the weight of any point from b to V the smallest, then add point b to the set V ; And so on, the current set V={a,b}, and then find another point c from the set UV so that the weight of any point from c to V is the smallest, at this time point c is added to the set V until all vertices All were added to V, and an MST was constructed at this time. Because there are N vertices, the MST has N-1 edges. Each time a point is added to the set V, it means that an MST edge is found.
* Kruskal's: With Kruskal's, we first arrange all edges from smallest to largest according to their weights, and then select each edge in order. If the two endpoints of this edge do not belong to the same set, then merge them until all the points belong to the same set Until the collection. As for how to merge into a collection, then here we can use a tool and search collection. The Kruskal algorithm is a greedy algorithm based on union search.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Write a 1-2 sentence overview that explains the general approach.

```markdown
1. Let visit[i][j] be the minimum cost from 0 to i with j times we have to use discount
2. Use Dijkstra's to find minimum cost from 0 to n-1. We keep a list of minimum cost for the current node and update
to find the edge with minimum cost in the priority queue.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

Because of how Djikstra's algorithm works, when we reach this node the first time, we will reach there with the lowest cost.  However, we may reach this node again with a highest cost, but more discount tickets, which can lead to a more optimal solution at the end.  If we ever come back to this node with the same or fewer discounts, the solution is not optimal.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def minimumCost(self, n, highways, discounts):
        pq = [(0, discounts, 0)]
        visited = set()
        
        # build an adjacency list
        adj = collections.defaultdict(list)
        for city1, city2, toll in highways:
            adj[city1].append((city2, toll))
            adj[city2].append((city1, toll))
        
        
        while pq:
            toll, d, city = heapq.heappop(pq)
            if (d, city) in visited: continue
            # add to visited set
            visited.add((d, city))
            
            if city==n-1: return toll
            
            # iterate through each node (cities) and check if they belong to the same connected component or not
            for nei, toll2 in adj[city]:
                if (d, nei) not in visited:
                    heapq.heappush(pq, (toll+toll2, d, nei))
                if d>0 and (d-1, nei) not in visited:
                    heapq.heappush(pq, (toll+toll2/2, d-1, nei))    
        
        return -1
```
```java
class Solution {
     public int minimumCost(int n, int[][] highways, int discounts) {
        
        Map<Integer, List<int[]>> graph = new HashMap<>();
        for (int[] highway: highways) {
            int city1 = highway[0], city2 = highway[1], toll = highway[2];
            if (!graph.containsKey(city1)) {
                graph.put(city1, new ArrayList<>());
            }
            graph.get(city1).add(new int[]{city2, toll});
            if (!graph.containsKey(city2)) {
                graph.put(city2, new ArrayList<>());
            }
            graph.get(city2).add(new int[]{city1, toll});
        }
        
        int[][] costs = new int[n][discounts+1];
        for (int[] c: costs) {
            Arrays.fill(c, Integer.MAX_VALUE);
        }
        costs[0][0] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (a[1] - b[1]));
        pq.offer(new int[]{0, 0, 0});
        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int city = cur[0], cost = cur[1], discount = cur[2];
            if (city == n-1)
                return cost;
            if (!graph.containsKey(city))
                continue;
            for (int[] nei: graph.get(city)) {
                int next = nei[0];
                int dCost = nei[1];
                if (cost+dCost < costs[next][discount]) {
                    pq.add(new int[]{next, cost+dCost, discount});
                    costs[next][discount] = cost+dCost;
                }
                if (discount < discounts && cost+dCost/2 < costs[next][discount+1]) {
                    pq.add(new int[]{next, cost+dCost/2, discount+1});
                    costs[next][discount+1] = cost+dCost/2;
                }
            }
        }
        return -1;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: `O(V+E + ElogE)`, `V+E` for building adjacency list, and `ElogE` for the process of Dijkstra's.
* **Space Complexity**: `O(V+E+ V*discounts)`