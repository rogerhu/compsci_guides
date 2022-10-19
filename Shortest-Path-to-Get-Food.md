## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/shortest-path-to-get-food/](https://leetcode.com/problems/shortest-path-to-get-food/)
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* **Similar Questions**: TBD

## 1. **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Does this problem give us a starting point?
This problem doesn't provide you a start point. We, then, need to find it by iterating over the grid to find the cell which value is `*`. The problem guarantee that there is only one `*`.
    
- What is the worst case scenario? 
We should visit each cell in the grid is the worst case scenario. Therefore, the time complexity is `O(mn)` where m, n is the row and column size of grid.
    
- When we find the food, what do we return?
Until we find the food, return the level.
    
    ```markdown
    HAPPY CASE
    Input: grid = [["X","X","X","X","X","X"],["X","*","O","O","O","X"],["X","O","O","#","O","X"],["X","X","X","X","X","X"]]
    Output: 3
    
    Input: grid = [["X","X","X","X","X"],["X","*","X","O","X"],["X","O","X","#","X"],["X","X","X","X","X"]]
    Output: -1
    
    Input: grid = [["X","X","X","X","X","X","X","X"],["X","*","O","X","O","#","O","X"],["X","O","O","X","O","O","X","X"],["X","O","O","O","O","#","O","X"],["X","X","X","X","X","X","X","X"]]
    Output: 6
    ```
    
## 2. M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
    - The use of BFS can help us keep track of what cells are already visited. Once a cell is visited mark it with `X`, so that during BFS when we see a cell with `X`, it is either obstacle or is already visited, so we can skip it.

## 3. P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
    - First, we need to find where to start.
    - Starting BFS with the help of deque `(i, j, cnt)`, explore 4 neighbors and increment `cnt` by 1
    - Mark visited point as `X` to avoid revisit
    - If `#` is met, return `cnt`

## 4. I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        public int getFood(char[][] grid) {
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
            m, n = len(grid), len(grid[0])
            q = collections.deque()
            for i in range(m):
                for j in range(n):
                    if grid[i][j] == '*': 
                        q.append((i,j, 0)); break
                if q: break        
            while q:
                x, y, cnt = q.popleft()
                if grid[x][y] == 'X': continue
                elif grid[x][y] == '#': return cnt
                grid[x][y] = 'X'
                for i, j in [(x + _x, y + _y) for _x, _y in [(-1, 0), (1, 0), (0, -1), (0, 1)]]:
                    if 0 <= i < m and 0 <= j < n and grid[i][j] != 'X':
                        q.append((i, j, cnt + 1))
            return -1
```
    
## 2. R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 3. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(NM)`, where M is the number of rows and N is the number of columns. We had to traverse the whole grid.
<br>
Space Complexity: `O(NM)`, where M is the number of rows and N is the number of columns. We needed a visited array to keep track of the visited cells in the grid.