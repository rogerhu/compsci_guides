## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/shortest-path-to-get-food/](https://leetcode.com/problems/shortest-path-to-get-food/)
* 💡 **Problem Difficulty:** Medium
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

- Does this problem give us a starting point?
  - This problem doesn't provide you a start point. We, then, need to find it by iterating over the grid to find the cell which value is `*`. The problem guarantee that there is only one `*`.
    
- What is the worst case scenario? 
  - We should visit each cell in the grid is the worst case scenario. Therefore, the time complexity is `O(mn)` where m, n is the row and column size of grid.
    
- When we find the food, what do we return?
  - Until we find the food, return the level.
    
```markdown
    HAPPY CASE
    Input: grid = [["X","X","X","X","X","X"],["X","*","O","O","O","X"],["X","O","O","#","O","X"],["X","X","X","X","X","X"]]
    Output: 3
    
    Input: grid = [["X","X","X","X","X"],["X","*","X","O","X"],["X","O","X","#","X"],["X","X","X","X","X"]]
    Output: -1
    
    Input: grid = [["X","X","X","X","X","X","X","X"],["X","*","O","X","O","#","O","X"],["X","O","O","X","O","O","X","X"],["X","O","O","O","O","#","O","X"],["X","X","X","X","X","X","X","X"]]
    Output: 6
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- BFS: The use of BFS can help us keep track of what cells are already visited. Once a cell is visited mark it with `X`, so that during BFS when we see a cell with `X`, it is either obstacle or is already visited, so we can skip it.
- DFS: We can use DFS to traverse the graph until we find a valid itinerary, ensuring we choose the lexicographically least itinerary.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
**General Idea:** Find the shortest path from one point to the other. 

1. First, we need to find where to start.
2. Starting BFS with the help of deque `(i, j, cnt)`, explore 4 neighbors and increment `cnt` by 1
3. Mark visited point as `X` to avoid revisit
4. If `#` is met, return `cnt`

⚠️ **Common Mistakes**

* Don't forget to check if the position you just dequeued has already been visited. It has the potential to be marked visited in the time between being queued and dequeued. Before you do the `grid[cur[0]][cur[1]] = 'X';` line, you need to do a check for `if(grid[cur[0]][cur[1]] == 'X')` and then break; if that is the case.

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        public int getFood(char[][] grid) {
            // create a queue 
            Queue<int[]> queue = new LinkedList<>();
            Set<String> visited = new HashSet<>();
            for (int i = 0; i < grid.length; i++) {
                for (int j = 0; j < grid[i].length; j++) {
                    if (grid[i][j] == '*') {
                        queue.offer(new int[]{i, j});
                        visited.add(i + "," + j);
                        break;
                    }
                }
            }
            int[] dx = new int[]{1, 0, 0, -1};
            int[] dy = new int[]{0, 1, -1, 0};
            int steps = 0;

            // apply bfs
            while (!queue.isEmpty()) {
                int size = queue.size();
                for (int i = 0; i < size; i++) {
                    int[] node = queue.poll();
                    if (grid[node[0]][node[1]] == '#') {
                        return steps;
                    }
                    for (int j = 0; j < 4; j++) {
                        int newx = node[0] + dx[j];
                        int newy = node[1] + dy[j];
                        if (newx >= 0 && newx < grid.length && newy >= 0 && newy < grid[0].length && grid[newx][newy] != 'X' && !visited.contains(newx + "," + newy)) {
                            queue.offer(new int[]{newx, newy});  
                            visited.add(newx + "," + newy);
                        }
                    }
                }
                steps++;
            }
            return -1;
        }
    }
```

```python
class Solution:
def getFood(self, grid: List[List[str]]) -> int:

    k = l = 0
    # initialize the truth table
    visited = [[False]*len(grid[0]) for p in range(len(grid))]

    # search for the starting position
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == '*':
                k,l = i,j   
                break
                
    # update the starting position's value in visited
    visited[k][l] = True
	
    # initialize queue with starting position and steps calculated
    queue = [(k,l, 0)]
    
    # start the BFS process from the starting position
    while queue != []:
        
        x,y,s = queue.pop(0)
        
	# runs in all directions
        for l,r in [(-1,0), (0,1), (1,0), (0,-1)]:
            
	    # checks for edge case
            if ((0 <= x+l < len(grid)) and (0 <= y+r < len(grid[0])) and not visited[x+l][y+r]):
                
		# found food, return the number of steps
                if grid[x+l][y+r] == '#':
                    return s+1
                
		# found empty space, add the queue and update visited
                if grid[x+l][y+r] == 'O':
                    queue.append((x+l, y+r, s+1))
                    visited[x+l][y+r] = True
    # could not find any food, exit the function
    return -1
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(NM)`, where M is the number of rows and N is the number of columns. We had to traverse the whole grid.
<br>
Space Complexity: `O(NM)`, where M is the number of rows and N is the number of columns. We needed a visited array to keep track of the visited cells in the grid.