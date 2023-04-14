## Problem Highlights

* 🔗 **Leetcode Link:** [Maximal Network Rank](https://leetcode.com/problems/maximal-network-rank)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Graph, Adjacency List
* 🗒️ **Similar Questions**: [Cheapest Flights Within K Stops
](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How can you efficiently check if there is a road connecting two different cities?
  -  Check neighbors
- Would it be efficient to use a matrix to save all the edges between vertex?
  - Yes
- If the edge is close to city_1 and city_2, then during counting, are they counted twice?
  - No, the common connection is counted only once.


```markdown
HAPPY CASE
Input: n = 5, roads = [[0,1], [0,3], [1,2], [1,3], [2,3], [2,4]]
Output: 5

Input: n = 8, roads = [[0,1], [1,2], [2,3], [2,4], [5,6], [5,7]]
Output: 5

EDGE CASE
Input: n = 4, roads = []
Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:

- Adjacency List: An adjacency list is efficient in terms of storage because we only need to store the values for the edges. For a sparse graph with millions of vertices and edges, this can mean a lot of saved space. It also helps to find all the vertices adjacent to a vertex easily. We can apply an adjacency list to count the number of edges for a pair of nodes and return the maximum of the counts.
- BFS/DFS: Not necessary. DFS is not a complete algorithm for infinitely deep graphs (it does not guarantee to reach the goal if there is any). Even if your graph is very deep but you have the prior knowledge that your goal is a shallow one, using DFS is not a very good idea.
BFS needs to keep all the current nodes in the memory.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a hashtable of size n which stores sets for each city. Members of the sets are cities which are directly connected to the city the set corresponds to. Then, examine all unique pairs (city_1, city_2) and sum up the edges of both cities. If there is an edge between both cities, the sum needs to be reduced by one since the edge is counted twice. We store the highest sum of edges and return it.

```markdown
1. Use a hashtable of size n which stores sets for each city. Members of the sets are cities which are directly connected to the city the set corresponds to.
2. Then, examine all unique pairs (city_1, city_2)
3. Sum up the edges of both cities.
4. If there is an edge between both cities, the sum needs to be reduces by 1 since the edge is counted 2x.
5. Then, store the highest sum of edges and return it.
```

⚠️ **Common Mistakes**

- A common mistake would be iterating 2 times, in other words, 2 for loops, to find the 1st and 2nd maximum values while traversing the loop.
- The 2 cities with most connections may not be necessarily connected with each other, and if they are connected, the common connection is counted only once.
 
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def maximalNetworkRank(self, n: int, roads: List[List[int]]) -> int:
        # use a set to store the neighbors
        city_to_cities = [ set() for i in range( n ) ]
        max_network_rank = 0
        # check each pair of cities, add their ranks together
        # For each (i, j) pair, if i is the neighbor of j or the vice versa,
        # we minus 1 on the rank
        for road in roads:
            city_to_cities[ road[0] ].add( road[1] )
            city_to_cities[ road[1] ].add( road[0] )
        for city_1 in range( n ):
            for city_2 in range( city_1 + 1, n ):
                network_rank = len( city_to_cities[city_1] ) + len( city_to_cities[city_2] )
                if ( city_1 in city_to_cities[city_2] ):
                    network_rank -= 1
                max_network_rank = max(max_network_rank, network_rank)
        return max_network_rank
```
```java
class Solution {
    
    private Map<Integer, Set<Integer>> graph;
    
    public int maximalNetworkRank(int n, int[][] roads) {
        
        graph = new HashMap<>();
        
        for(int i = 0; i < n; i += 1) graph.put(i, new HashSet<>());
        
        for(int[] road : roads) {
            int a = road[0], b = road[1];
            // edge insertions
            graph.get(a).add(b); 
            graph.get(b).add(a);
        }
        
        int maxNetworkRank = 0;
        
        // trying all possible pairs of cities
        for(int i = 0; i < n; i += 1) {
            for(int j = i + 1; j < n; j += 1) {
                int city_a = i, city_b = j, networkRank = 0;
                int degreeA = graph.get(city_a).size();
                int degreeB = graph.get(city_b).size();
                if(graph.get(city_a).contains(city_b)) {
                    networkRank = degreeA + degreeB - 1;
                } else {
                    // not connected directly adjacently. Might be in the same component or in different components.
                    networkRank = degreeA + degreeB;
                }
                maxNetworkRank = Math.max(maxNetworkRank, networkRank);
            }
        }
        return maxNetworkRank;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `V` represents the number of vertices.
Assume `E` represents the number of edges

* **Time Complexity**: O(E + V^2) The worst case runtime is O(V^2). This will happen when all nodes have the same amount of edges.
* **Space Complexity**: O(E) accounting for the use of a hash table to store the neighbors of each city
