## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/minimum-knight-moves](https://leetcode.com/problems/minimum-knight-moves) 
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: TBD


## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How many neighbors do we have at each point?
  - At each point, we can have 8 neighbors. We need to consider only neighbors to the right (or left) of the given point. This means for `(0,0)`, we need to consider only 4 neighbors to its right to get to `(x,y)`.
    
- How can we make sure neighbors are not added multiple times?
  - You can use a data structure like a hashMap to make sure neighbors are not added multiple times.
    
- How can we reduce this graph to find the minimum number of moves?
  - Each position on the board can be thought of as a node. Edges can be thought of as possible moves for a knight from one position to other. This leads to an undirected uniform weighted graph. Thus the shortest distance to the target position is our answer.
    
```markdown
    HAPPY CASE
    Input: x = 2, y = 1
    Output: 1
    
    Input: x = 5, y = 5
    Output: 4
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- BFS:  Use BFS to find the shortest path with optimizations:
    1. don't go down visited paths by using a set to keep track of visited paths.
    2. pruning out directions that head away from the target `x,y`. 
    
    At each point, we've got 8 coordinates (8 neighbors) that we can move to in any given location. As with normal BFS, we build a queue, keep track of what we've visited, and explore our 8 neighbors. Observe that our movements are completely symmetric. Whether our destination was `(x,y)`, `(x,-y)`, `(-x,y)`, or `(-x,-y)`, our answer would be the exact same. So what we can do is restrict our search space to just one quadrant (namely the 1st quadrant where `x,y` are both positive). When exploring our neighbors, if we ever fall outside of quadrant 1, ignore that neighbor. 

- DFS: We can use DFS to traverse the graph until we find a valid itinerary, ensuring we choose the lexicographically least itinerary.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
1. Start from `{0,0}`. The distance to `{0,0}` here is `0`. Add this position to queue.
2. Keep performing BFS from each point present in the queue. At each step poll a point and explore all 8 possible tiles where the knight can land and add those points to the queue if not visited.
3. Thus each point reaches one more hop to the neighbor. And eventually reaches the target node.

⚠️ **Common Mistakes**

* Avoid visiting the same cell again. You can realize that the problem is symmetric, i.e. all 4 of (±x, ±y) will have the same solution, so restrict the solution space to the first quadrant. It's possible to reach (x, y) by going 1 step out of Q1 then returning to Q1, so max you can go out of Q1 is 2 steps in either direction.


## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
class Solution {
    public int minKnightMoves(int x, int y) {
        // the offsets in the eight directions
        int[][] offsets = {{1, 2}, {2, 1}, {2, -1}, {1, -2},
                {-1, -2}, {-2, -1}, {-2, 1}, {-1, 2}};
        boolean[][] visited = new boolean[607][607];

        Deque<int[]> queue = new LinkedList<>();
        queue.addLast(new int[]{0, 0});
        int steps = 0;

        while (queue.size() > 0) {
            int currLevelSize = queue.size();
            // iterate through the current level
            for (int i = 0; i < currLevelSize; i++) {
                int[] curr = queue.removeFirst();
                if (curr[0] == x && curr[1] == y) {
                    return steps;
                }

                for (int[] offset : offsets) {
                    int[] next = new int[]{curr[0] + offset[0], curr[1] + offset[1]};
                    // align the coordinate to the bitmap
                    if (!visited[next[0] + 302][next[1] + 302]) {
                        visited[next[0] + 302][next[1] + 302] = true;
                        queue.addLast(next);
                    }
                }
            }
            steps++;
        }
        // move on to the next level
        return steps;
    }
}
```

```python
class Solution:
    def minKnightMoves(self, x: int, y: int) -> int:
        # the offsets in the eight directions
        offsets = [(1, 2), (2, 1), (2, -1), (1, -2),
                   (-1, -2), (-2, -1), (-2, 1), (-1, 2)]

        def bfs(x, y):
            visited = set()
            queue = deque([(0, 0)])
            steps = 0

            while queue:
                curr_level_cnt = len(queue)
                # iterate through the current level
                for i in range(curr_level_cnt):
                    curr_x, curr_y = queue.popleft()
                    if (curr_x, curr_y) == (x, y):
                        return steps

                    for offset_x, offset_y in offsets:
                        next_x, next_y = curr_x + offset_x, curr_y + offset_y
                        if (next_x, next_y) not in visited:
                            visited.add((next_x, next_y))
                            queue.append((next_x, next_y))

                # move on to the next level
                steps += 1

        return bfs(x, y)
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity:** O(xy), where (x, y) are the coordinates of the target. 
<br>
* **Space Complexity:** O(xy), where (x, y) are the coordinates of the target. 
