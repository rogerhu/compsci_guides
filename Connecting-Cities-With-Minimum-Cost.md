## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/connecting-cities-with-minimum-cost/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Dijkstra's, Union Find, Minimum Spanning Tree
* 🗒️ **Similar Questions**: [Minimum Cost to Reach City With Discounts](https://leetcode.com/problems/minimum-cost-to-reach-city-with-discounts/)
    
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
- How does adding edges to a data structure like a heap affect the time complexity?
  - As you're adding all edges to the heap, it affects the time complexity. You need to use a heap that can bubble up keys already inside it, and store the vertex's min edge weights to the connected set.
   
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

* Union Find: At every single step, we use the shortest/cheapest route. The only issue will be, we might construct redundant routes. We use
union find to avoid this issue. After we finish constructing the route, we can do one last check to make sure all the nodes are in this one single set. We can greedily pick the smallest edge at each iteration to connect cities together. Then, in the end, the result will be the smallest.
* Dijkstra’s: We essentially need to find the minimum distance to get from node 0 to node n - 1 in an undirected weighted graph. What algorithm should we use to do this? Since, Dijkstra’s algorithm that seeks the minimum weighted vertex on every iteration, so the original Dijkstra’s algorithm will output the first path but the result should be the second path as it contains minimum number of edges.
* Prim's: With Prim's, the algorithm resembles Dijkstra's algorithm. The MST is constructed starting from a single vertex and adding in new edges to the MST that link the partial tree to a new vertex outside of the MST
* Kruskal's: With Kruskal's, we first arrange all edges from smallest to largest according to their weights, and then select each edge in order. If the two endpoints of this edge do not belong to the same set, then merge them until all the points belong to the same set Until the collection. As for how to merge into a collection, then here we can use a tool and search collection. The Kruskal algorithm is a greedy algorithm based on union search.
* BFS: We can use a queue for traversal. BFS can help us avoid processing a node more than once, because we can divide the vertices into two categories: Visited and Not visited. A boolean visited array is used to mark the visited vertices. For simplicity, it is assumed that all vertices are reachable from the starting vertex. 

Note: Which one is better --- Prims or Kruskal?
If the graph consists of lots of edges, i.e. for dense graphs, then Prim’s Algorithm is more efficient than Kruskal’s algorithm.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Write a 1-2 sentence overview that explains the general approach.

```markdown
1. Implement the Union find first as usual with Path Compression using forest
2. This is a greedy Algorithm, the way it works is every time you need to find the minimum cost from the connections and check if including this Edge would cause a loop or cycle, which is where our Union Find would come Handy.
3. Union Find Algo finds the parent of two cities and checks if they are not the same, because if they are then you are already forming a cycle that would violate the minimum spanning tree property so then out check helps over there.
4. After that, you can append the cost and in the end make these cities a new set using Union
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

Because of how Djikstra's algorithm works, when we reach this node the first time, we will reach there with the lowest cost.  However, we may reach this node again with a highest cost, but more discount tickets, which can lead to a more optimal solution at the end.  If we ever come back to this node with the same or fewer discounts, the solution is not optimal.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def minimumCost(self, N, connections):
        parent = [-1] * (N+1)
        forest = [1] * (N+1)
        self.n = N
        mst = []
        res = 0
        
        for city1, city2, cost in sorted(connections, key = lambda x: x[2]):
            if self.find(parent, city1) != self.find(parent, city2):
                mst.append((city1, city2))
                res += cost
                if len(mst) == N:
                    break
                self.Union(forest, parent, city1, city2)
        return res if self.n==1 else -1
                
    
    def find(self, parent, i):
        while parent[i] != -1:
            i = parent[i]
        return i
    
    def Union(self,forest, parent, x, y):
        set1 = self.find(parent, x)
        set2 = self.find(parent, y)
        
        if set1 != set2:
            if forest[set1] < forest[set2]:
                parent[set1] = set2
                forest[set2] += forest[set1]
            else:
                parent[set2] = set1
                forest[set1] += forest[set2]
        self.n-=1
```
```java
class Solution {
    public int minimumCost(int N, int[][] connections) {
        // before the union, each city is considered as isolated, not-connected node, so there should be N unions at first 
	// 1 indexed, so need N+1 length
        UnionFind uf = new UnionFind(N + 1);
        // sort connections in a way that the cost is in ascending order 
        Arrays.sort(connections, new Comparator<int[]>(){
            public int compare(int[] a, int[] b){
                return a[2] - b[2];
            }                 
        });
       
        int cost = 0;
        for(int i = 0; i < connections.length; i++) {
            int x = connections[i][0], y = connections[i][1];
            if(!uf.find(x, y)) {
                uf.union(x, y);
                cost += connections[i][2];
            }
        }
        return connections.length < N - 1 ? -1 : cost;
    }
    
    // each time we connect a city into the union, there will be one less isolated city
    class UnionFind {
        int[] array;
        public UnionFind(int size) {
            array = new int[size];
            for(int i = 0; i < size; i++) {
                array[i] = i;
            }
        }
        
        public int root(int i) {
            while(i != array[i]) {
                i = array[i];
            }
            return i;
        }
        
        public boolean find(int i, int j) {
            return root(i) == root(j);
        }
        
        public boolean union(int i, int j) {
            if(find(i, j)) return false;
            array[root(i)] = j;
            return true;
        }
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