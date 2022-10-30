## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/swim-in-rising-water/](https://leetcode.com/problems/swim-in-rising-water/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search, Breadth-First Search
* 🗒️ **Similar Questions**: [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we think of the grid as a weighted graph?
  - Yes, to solve this problem, we have to think about the grid as a weighted graph. Each cell is a node. An edge is a connector between two of such nodes. The weight of an edge is calculated as the maximum value of two nodes connected by that edge. That way we reduced the problem to graph traversal problem with some constraints. We no longer want to search for the shortest path, but a path with the minimum-maximum value.
    
- Where do I start on the grid?
  - Start with `(0,0)`corner. On each moment of time we choose node with smallest value to visit. In this way when we reached `(N-1, N-1)`corner, the answer will be the maximum of visited cells so far.
    
```markdown
    HAPPY CASE
    Input: grid = [[0,2],[1,3]]
    Output: 3
    
    Input: grid = [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
    Output: 16
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

**General Idea:** Perform a BFS to check whether it is possible where we can only walk in squares that are at most `T`.
    
1. Take a priority queue to store grid
2. Take a visited[r][c] to keep track of visited nodes
3. Set result = 0
4. Add (grid[0][0], 0, 0) to priority_queue and mark it as visited
5. While priority_queue is not empty, take top element from priority_queue (max, r, c)
6. Mark it as visited
7. If r == N-1 and c = N-1, return result
8. For each of the valid neighbors (in 4 directions) and not visited, mark this node as visited
9. result = max of its own values and the one from max value of item popped
10. Add to priority queue (grid[newRow][newColumn], newRow, newColumn)
11. Return result

⚠️ **Common Mistakes**

* The brute force solution may not garner the best time `T`. We can use either Dijkstra's, or binary search for the best time `T` where you can reach the end if you only step on squares at most `T`.
    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        int[][] directions = new int[][]{{0,1}, {0,-1}, {1,0}, {-1,0}};
        public int swimInWater(int[][] grid) {
            int rowLen = grid.length;
            int colLen = grid[0].length;

            // take a priority queue to store grid
            // add (grid[0][0], 0, 0) to priority_queue
            Queue<int[]> pq = new PriorityQueue<>((a,b) -> Integer.compare(a[2],b[2]));
            pq.add(new int[]{0, 0, grid[0][0]});
            
            int result = grid[rowLen-1][colLen-1];
            
            // mark as visited
            boolean[][] visited = new boolean[rowLen][colLen];
            
            while(pq.isEmpty()==false) {
                int[] currentCell = pq.poll();
                result = Math.max(result, currentCell[2]);
                visited[currentCell[0]][currentCell[1]] = true;
                
                // for each of the valid neighbors (in 4 directions) and not visited, mark this node as visited
                // result = max of its own values and the one from max value of item popped
                if(currentCell[0]==rowLen-1 && currentCell[1]==colLen-1) {
                    break;
                }
                
                for(int[] dir : directions) {
                    int newRow = currentCell[0] + dir[0];
                    int newCol = currentCell[1] + dir[1];
                    
                    if(newRow < 0 || newRow >= rowLen || newCol < 0 || newCol >= colLen) {
                        continue;
                    }
                    
                    if(visited[newRow][newCol]) {
                        continue;
                    }
                    
                    // add to priority queue 
                    pq.add(new int[]{newRow, newCol, grid[newRow][newCol]});
                }
            }
            return result;
        }
    }
```

```python
    class Solution:
        def swimInWater(self, grid: List[List[int]]) -> int:
            directions = [(0, 1), (0, -1), (-1, 0), (1, 0)]
            N = len(grid) 
            # take a visited[r][c] to keep track of visited nodes
            visited = [[False] * N for _ in range(N)]
   
            # take a priority queue to store grid
            pq = []
            ans = 0

            # add (grid[0][0], 0, 0) to heap
            heapq.heappush(pq, (grid[0][0], 0, 0))
    

            # while priority queue is not empty, take top element from priority_queue (max, r, c)
            while pq:
                time, r, c = heapq.heappop(pq)
                if visited[r][c]:
                    continue

                # mark as visited
                visited[r][c] = True
                ans = max(ans, time)
    
                if r == N - 1 and c == N - 1:
                    break
    
                for dir_r, dir_c in directions:
                    rr, cc = r + dir_r, c + dir_c
                    if not self._isValid(rr, cc, N, visited):
                        continue
                    heapq.heappush(pq, (grid[rr][cc], rr, cc))
    
            return ans
    
        # helper function to check for each of the valid neighbors (in 4 directions) and not visited, mark this node as visited
        def _isValid(self, r, c, N, visited):
            return r >= 0 and r < N and c >= 0 and c < N and not visited[r][c]
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(r*c* logn)`, where `r`, `c` = rows and columns in grid and `n` is the max cell elevation found in the grid
<br>
Space Complexity: `O(r * c)`, where `r`, `c` = rows and columns in grid and `n` is the max cell elevation found in the grid