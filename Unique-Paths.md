## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/unique-paths/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What function do we write to return the number of unique paths?
  - Write a function that returns the number of unique paths to get from A (top-left corner) to B (bottom-right corner) for any m x n grid
- How do we represent a 2D grid?
  - Use a 2-D list to represent the m x n grid

Run through a set of example cases:

```markdown
HAPPY CASE
Input:  m = 8, n = 3
Output: 24

Input:  m = 6, n = 8
Output: 792

EDGE CASE
Input:  m = 1, n = 1
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


* From the description we know that, the robot can only move down or right, which means, if the robot is now in position (x,y), then the position before this step must be either (x-1,y) or (x, y-1). The problem is to count all the possible paths from top left to the bottom right of the m*n matrix with the constraints that from each cell you can either move only to right or down. So, we can use recursion to solve the given problem.Since current position is only from these two previous positions, the number of possible paths that the robot can reach this current position (x,y) is the sum of paths from (x-1, y) and (x, y-1).

Backtracking can also be used to make sure that the path is simple and can be done without cycles. Backtracking can keep track of cells involved in the current path in a matrix, and before exploring any cell, ignore the cell if already covered in the current path.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** There is only one way to reach the left-most edge and there is only one way to reach the bottom-most edge. The robot can only move either down or right. For each target cell that the robot wants to reach, it is either from the left cell of the target or from the top cell of the target. When reach the bottom-right corner, the number in the bottom-right corner will be the answer.


```markdown
1) Find the base case by initializing the first row and first column of the 2D list with 1.

2) Find the pattern by summing up the target’s left cell and up cell to get the total path to reach the target cell. 

3) When reach the bottom-right corner, the number in the bottom-right corner will be the answer.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

* A common mistake is not initializing the grid to 1. In the beginning, we’ll initialize the grid values to 1. These values will remain for the first row and first column. This is because, for each cell in the first row and column of a grid, there is only one way to reach that cell. For instance, to reach the blue cell in the grid above, the only path is to move two steps to the right from point A. Similarly, to reach the red cell, the only path is to move one step down.

* Another way to solve this problem is by recursion. However, the time complexity of the above recursive solution is exponential. Also, we notice that the problem satisfies both the conditions of a dynamic programming problem. Hence, the re-computations of the same subproblems can be avoided by constructing a temporary array count[][] in a bottom-up manner using a recursive formula.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def __init__(self):
        self.res = dict()

    def uniquePaths(self, m, n):
        # Check for out of bound matrix cell
        if m <= 0 or n <= 0:
            return 0
        # There is only one path to reach origin cell of matrix
        if m == 1 and n == 1:
            return 1
        
        # p(m, n) = Number of unique paths to reach cell (m, n) 
        # p(m, n) = p(m-1, n) + p(m, n-1)   Only two ways to move right and down.
        if (m, n) not in self.res:
            self.res[(m, n)] = self.uniquePaths(m-1, n) + self.uniquePaths(m, n-1)
        return self.res[(m, n)]
```
```java
class Solution {
    public int uniquePaths(int m, int n) {
        // grid MxN where each position is a number of pathes we need to reach this position
        int[][] dp = new int[m+1][n+1];
        
        // traverse through all of cells in the grids
        for(int i = 0; i <= m; i++) {
            for(int j = 0; j <= n; j++) {
                // set default value
                dp[i][j] = 0;
                
                // if there is a border and we cannot get cell at the top
                if(i-1 > 0) {
                   dp[i][j] += dp[i-1][j];
                }
                // if there is a border and we cannot get cell at the left
                if(j-1 > 0) {
                   dp[i][j] += dp[i][j-1];
                }
                // number of paths should be at least zero
                if(dp[i][j] == 0) {
                    dp[i][j] = 1;
                }
            }
        }
        
        return dp[m][n];
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N*M) since we need to touch each element a single time in grid
* **Space Complexity**: O(N*M) since we need to initialize a 2D grid containing M*N elements