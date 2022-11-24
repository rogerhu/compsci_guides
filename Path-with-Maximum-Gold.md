## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/path-with-maximum-gold/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Backtracking
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What if the grid point is a 0?
  - If (i, j) is out of the bound, or grid[i][j] == 0, or visited already, return current sum.
- How do we explore the "grid?"
  - Since 1 <= grid.length, grid[i].length <= 15 is a very small magnitude, we can use DFS to explore the grid

Run through a set of example cases:

```markdown
HAPPY CASE
Input: grid = [[0,6,0],[5,8,7],[0,9,0]]
Output: 24

Input: grid = [[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]
Output: 28

Input: [[1,1,1],[1,1,1],[1,1,1]]
Output: 9

Input: [[1,0,1],[1,0,1],[1,0,1]]
Output: 3

EDGE CASE
Input: [[1,0,90],[0,0,0],[0,0,1]]
Output: 90
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


* Since we don't know where the "golden path" is, and we don't have a good rule to traverse the grid optimally (unless you resort to some other graph algorithm), we can do a DFS testing all options and come out with the maximum sum. We’ll need to apply DFS on every single cell on the grid that have a value > 0. Apply recursion and backtracking to mark node as currently visited while traversing the grid. Return the maximum count from all four sides.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Amount of gold is positive, so we cover maximum cells of maximum values under given constraints. In every move, we move one step toward right side. So we always end up in last column. If we are at the last column, then we are unable to move right. We can perform a simple DFS starting with each cell with gold and find the maximum possible gold considering each step as a new start.

```markdown
1) for each cell in the grid, start DFS. DFS state is current index and it should return starting at current index, the maximum gold it could get.
2) if current index is illegal, return 0. Otherwise, return grid[i][j] + for each direction
3) do DFS, get the maximum among 4 directions, and return.
4) before return, backtracking grid[i][j] to its original value.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def getMaximumGold(self, grid: List[List[int]]) -> int:
        directions = [(1, 0), (-1,0), (0, 1), (0,-1)]
        m, n = len(grid), len(grid[0])
        self.res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] != 0:
                    visited = set()
                    self.dfs(i, j, 0, grid, visited, directions)
        return self.res
    
    
    
    def dfs(self, x, y, cur, grid, visited, directions):
        m, n = len(grid), len(grid[0])
        # 1. restraints
        if x < 0 or x > m-1 or y < 0 or y > n-1 or grid[x][y]==0 or (x,y) in visited:
            return
        
        # 2. condition for adding new result
        self.res = max(self.res, cur+grid[x][y])

        # 3. main body for backtrack searching.
        visited.add((x, y)) # mark current positio nas visited
        for dx,dy in directions:
            self.dfs(x+dx, y+dy, cur+grid[x][y], grid , visited, directions)
        visited.remove((x, y)) # unmark current position since we return
```
```java
class Solution {
    public int getMaximumGold(int[][] grid) {
    
        int m = grid.length;
        int n = grid[0].length;
        
        int res = 0; //Intialize the result. we will be updating it accordingly
        
		// Loop over all the valid points i.e. non zero places
        for(int i=0; i<m; i++){
            
            for(int j=0; j<n; j++){
                
                if(grid[i][j] != 0){
                    res = Math.max(res, dfs(grid, i, j)); // Call the magical function that gives you the max gold if you start from this point and update res accordingly
                }
                
            }
            
        }
        return res;
    }
    
    
    public int dfs(int[][] grid, int i, int j){
        
        if(i<0 || i>= grid.length || j<0 || j>= grid[0].length || grid[i][j] == 0) return 0; // return 0 if invalid location or the place has 0 gold or if the place is already visited
        
        int val = grid[i][j]; // save the value of this location into a temp var
        
        grid[i][j] = 0; // set the value of current location as 0 to mark it as visited
        
        int left = dfs(grid, i, j-1); // traverse left
        int right = dfs(grid, i, j+1); // traverse right
        int up = dfs(grid, i-1, j); // traverse up
        int down = dfs(grid, i+1, j); // traverse down
        
        grid[i][j] = val; // reset the value from temp var
        
        return grid[i][j] + Math.max(Math.max(Math.max(left,right),up),down); // get the max of all the four directions and add current value and return
        
    }
    
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(M * N), as we touched every single element a single time
* **Space Complexity**: O(M * N), since we used extra space from our visited array and recursion depth
