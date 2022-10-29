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
    
    - Use BFS to find the shortest path with optimizations:
    1. don't go down visited paths by using a set to keep track of visited paths.
    2. pruning out directions that head away from the target `x,y`. 
    
    At each point, we've got 8 coordinates (8 neighbors) that we can move to in any given location. As with normal BFS, we build a queue, keep track of what we've visited, and explore our 8 neighbors. Observe that our movements are completely symmetric. Whether our destination was `(x,y)`, `(x,-y)`, `(-x,y)`, or `(-x,-y)`, our answer would be the exact same. So what we can do is restrict our search space to just one quadrant (namely the 1st quadrant where `x,y` are both positive). When exploring our neighbors, if we ever fall outside of quadrant 1, ignore that neighbor. 
    
## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
    - Start from `{0,0}`. The distance to `{0,0}` here is `0`. Add this position to queue.
    - Keep performing BFS from each point present in the queue. At each step poll a point and explore all 8 possible tiles where the knight can land and add those points to the queue if not visited.
    - Thus each point reaches one more hop to the neighbor. And eventually reaches the target node.

⚠️ **Common Mistakes**

* 


## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        public int minKnightMoves(int x, int y) {
            Map<String, Integer> map = new HashMap<>();
            Queue<int[]> q = new LinkedList<>();
            int res = 0;
            int[][] offsets = new int[][]{{-1,-2}, {-1,2}, {1,-2}, {1,2}, {-2,-1}, {-2,1}, {2,-1}, {2,1}};
            q.offer(new int[]{0,0});
            
            while(!q.isEmpty()) {
                int size = q.size();
                for(int k=0; k<size; k++) {
                    int[] curr = q.poll();
                    if(curr[0] == Math.abs(x) && curr[1] == Math.abs(y)) return res;
    
                    for(int[] offset: offsets) {
                        if(curr[0] < -2 || curr[1] < -2) continue;
                        int xnew = curr[0]+offset[0];
                        int ynew = curr[1]+offset[1];
                        
                        if(map.get(xnew+","+ynew) != null) continue;
                        q.offer(new int[]{xnew, ynew});
                        map.putIfAbsent(xnew+","+ynew, res);
                    }
                }
                res++;
            }
            
            return res;
        }
    }
```

```python
    class Solution:
        def minKnightMoves(self, x: int, y: int) -> int:
            dirs = [(1, 2), (2, 1), (1, -2), (2, -1), (-1, 2), (-2, 1)]
            
            queue = []
            curr = [0, 0]
            visited = []
            queue.append((curr, 0))
            visited.append(curr)
            x, y = abs(x), abs(y)
         
            while queue:
                coord, count = queue.pop(0)
                if coord[0] == x and coord[1] == y:
                    return count
                    
                for dir in dirs:
                    next_coord = [coord[0] + dir[0], coord[1] + dir[1]]
                    if next_coord not in visited and x + 2 >= next_coord[0] >= -1 and y + 2 >= next_coord[1] >= -1:
                        next_count = count + 1
                        visited.append(next_coord)
                        queue.append((next_coord, next_count))
               
            return -1
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
