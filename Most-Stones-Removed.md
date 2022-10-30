## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search
* 🗒️ **Similar Questions**: [Battleships in a Board](https://leetcode.com/problems/battleships-in-a-board/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How many stones can there be on the plane?
  - There can be up to 1000 stones.

- Could there be no stones?
  - Yes, there could be no stones.

- What if no stones can be removed?
  - Then the result should be zero.
    
    ```markdown
    HAPPY CASE
    Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
    Output: 5
    
    EDGE CASE
    Input: stones = [[0,0]]
    Output: 0
    ```
    
## 2: M-atch
    
> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

    For graph problems, some things we want to consider are:
    
- BFS/DFS: We can possible use BFS to traverse the stones graph, but it will lead to a difficult solution. We can use DFS to traverse the graph and use it to count the number of connected components. We can use a map to store the edges in the graph to lookup rows by index to reduce average runtime complexity.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. We can use union find to count the number of islands by adding each stone to union-find set, and counting number of sets.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can first create an empty disjoint set, add each stone in a loop, and at each iteration compute the number of sets. At the end, # of stones - # of sets.
    
1) Create a disjoint set. 
2) For each stone
      a) Add the coordinate x, ~y (i.e. -(y-1)) to the set
      b) Update number of sets
3) Return number of stones - number of sets
   
    
⚠️ **Common Mistakes**
    
* Some people may want to sort the array first, but there are faster solutions if we avoid sorting. We can reduce runtime complexity from O(N*log(N)) to O(N+M) if we avoid sorting either of the arrays. Though, we can sort for O(1) space complexity.


## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```python
class Solution(object):
    def removeStones(self, stones):
        stones = list(map(tuple, stones))
        s = set(stones)
        d = collections.defaultdict(list)
        for i,j in s:
            d[i].append(j)
            d[j].append(i)
        
        def dfs(i,j):
            for y in d[i]: # find all points in x=i
                if (i,y) not in s: continue
                s.remove((i,y))
                dfs(i,y)
            for x in d[j]: # find all points in y=j
                if (x,j) not in s: continue
                s.remove((x,j))
                dfs(x,j)
        
        n = len(s)
        res = 0
        for i,j in stones:
            if (i,j) not in s: continue
            s.remove((i,j))
            dfs(i,j)
            # n-len(s) represent the length of graph, e.g. the number of element removed through dfs
            res += n - len(s) - 1 
            n = len(s)
        return res
```
    
```java
class Solution {
    // return true if stone a and b shares row or column
    boolean shareSameRowOrColumn(int[] a, int[] b) {
        return a[0] == b[0] || a[1] == b[1];
    }
    
    void dfs(int[][] stones, List<Integer>[] adj, int[] visited, int src) {
        // mark the stone as visited
        visited[src] = 1;
        
        // iterate over the adjacent, and iterate over it if not visited yet
        for (int adjacent : adj[src]) {
            if (visited[adjacent] == 0) {
                dfs(stones, adj, visited, adjacent);
            }
        }
    }
    
    int removeStones(int[][] stones) {
        // create adjacency list to store edges
        List<Integer>[] adj = new ArrayList[stones.length]; 
        for (int i = 0; i < stones.length; i++) {
            adj[i] = new ArrayList<>();
        }
        
        for (int i = 0; i < stones.length; i++) {
            for (int j = i + 1; j < stones.length; j++) {
                if (shareSameRowOrColumn(stones[i], stones[j])) {
                    adj[i].add(j);
                    adj[j].add(i);
                }
            }
        }
        
        // create array to mark visited stones
        int[] visited = new int[stones.length];
        // counter for connected components
        int componentCount = 0;
        for (int i = 0; i < stones.length; i++) {
            if (visited[i] == 0) {
                // If the stone is not visited yet,
                // Start the DFS and increment the counter
                componentCount++;
                dfs(stones, adj, visited, i);
            }
        }
        
        // return the maximum stone that can be removed
        return stones.length - componentCount;
    }
};
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time complexity: O(N^2 + E)
Building the graph will need O(N^2) as needed to traverse over all pairs of stones. During the DFS traversal, each stone only is visited once. This is because we mark each stone as visited as soon as we see it, and then we only visit stones that are not marked as visited. In addition, when we iterate over the edge list of each stone, we look at each edge once. 

Space complexity: O(N + E)
Building the adjacency list will take O(E) space. To keep track of visited vertices, an array of size O(N), is required. Also, the run-time stack for DFS will use O(N) space.