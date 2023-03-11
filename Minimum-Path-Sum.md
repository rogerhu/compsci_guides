## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/minimum-path-sum/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Unique Paths](https://leetcode.com/problems/unique-paths/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are two possible paths?
  - There are only 2 ways to reach (x, y) i.e either from (x-1, y) and (x, y-1). so, take the route which gives the minimum. Therefore at every cell, we will make the choice to move which costs are less.
- 

   
```markdown
EXAMPLES

Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
Output: 7

Input: grid = [[1,2,3],[4,5,6]]
Output: 12

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

The key to this DP problem is subproblems. Whenever you run into a situation where it seems like you don't know how to solve the problem because you don't know which decision to take at each point in the code, it means use DP to enumerate all the decisions and choose the best one. This problem is easier because the problem tells you the two decisions you have at each recursion is going down or going right. In other problems, it could involve iterating over an entire array and choosing the best answer from those subproblems. This is what dynamic programming is all about - trying to figure out what are my decisions at each subproblem.

In this problem, we start at coordinate (0,0) and we try to decide whether it is better to go right or down. We don't know, so we try both. We recurse startx+1 in one branch and starty+1 in another branch. We choose the minimum of both because we don't know which one will be better! If we ever iterate over the bounds of m x n, we know that is not a viable path, so we can return an absurdly large number to rule it out. We only ever return a real number from our base case is we land on the destination node, which is the bottom right node minMatrix[m-1][n-1].

* Would a Greedy approach work here?
  - As we have to return the minimum path sum, the first approach that comes to our mind is to take a greedy approach and always form a path by locally choosing the cheaper option.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1. We first create an m by n minMatrix. This matrix represents the minimum distance to travel starting from square (i, j) to get to corner element (m-1, n-1). At the end of this algorithm we really are looking for (0,0). Pause and think about this.
2. We recursively call minPathSum to populate minMatrix starting from (0,0). Note, this will fully populate minMatrix.
3. We check bounds at the top of the recursion OR if the grid element located at (i,j) has been visited in the past. Note, we leverage the fact that the grid has ONLY non-negative integers here, so during recursion we can use -1 as a flag to indicate if it has been visited.
4. We check if minMatrix already contains a non-negative entry at (i,j). This leverages the fact that if this is populated then it must have already gone through the recursion to identify this value. 
5. We perform the basic recursion:
We first pull the value out of the grid to save it in the current stack/frame for the recursion

We mark the (i,j) cell as visited in the grid (so we don't visit it again)

We recurse on i+1 (down) and j+1 (right). This will define our entries for minMatrix for (i+1, j) and (i, j+1).

Next we check boundaries to ensure that i+1 or j+1 does not fall out of the grid.

minMatrix[i][j] is the current value plus the minimum found between adjacent cells (down or right) when we are not along the edge of the grid.

minMatrix[i][j] is the current value plus the minimum found from the down cell (this is the case when you are on the far right wall of the grid).

minMatrix[i][j] is the current value plus the minimum found from the right cell (this is the case when you are along the bottom of the grid).

minMatrix[i][j] is the current value when we are at the corner of the matrix (down and left aren't defined).
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

The Greedy approach will not give us the correct answer. Whenever we are making a local choice, we may tend to choose a path that may cost us way more later.

Therefore, the other alternative left to us is to generate all the possible paths and see which is the path with the minimum path sum. To generate all paths we will use recursion.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution {
    public int minPathSum(int[][] grid) {
       
        int[][] minMatrix = new int[grid.length][grid[0].length];
        for(int i=0;i<grid.length;i++) {
            for (int j=0;j<grid[0].length;j++) {
                minMatrix[i][j] = -1;
            }
        }
        
        minPathSum(grid, minMatrix, 0, 0);
        
        return minMatrix[0][0];
    }
    
    private void minPathSum(int[][] grid, int[][] minMatrix , int i, int j) {
        if (i >= grid.length || j >= grid[i].length || grid[i][j] == -1) return; 
        else if (minMatrix[i][j] >= 0) return;  
        else {
            int value = grid[i][j];
            grid[i][j] = -1; // visited
            minPathSum(grid, minMatrix, i+1, j);
            minPathSum(grid, minMatrix, i, j+1);
            grid[i][j] = value;
            if (i + 1 < grid.length && j + 1 < grid[0].length)
                minMatrix[i][j] = value + Math.min(minMatrix[i+1][j], minMatrix[i][j+1]);
            else if (i + 1 < grid.length)
                minMatrix[i][j] = value + minMatrix[i+1][j];
            else if (j + 1 < grid[0].length)
                minMatrix[i][j] = value + minMatrix[i][j+1];
            else
                minMatrix[i][j] = value;
        }
    }
}
```
```java
public class Solution {
    public int minPathSum(int[][] grid) {
        int[][] minMatrix = new int[grid.length][grid[0].length];
        for (int i = grid.length - 1; i >= 0; i--) {
            for (int j = grid[0].length - 1; j >= 0; j--) {
                if(i == grid.length - 1 && j != grid[0].length - 1)
                    minMatrix[i][j] = grid[i][j] +  minMatrix[i][j + 1];
                else if(j == grid[0].length - 1 && i != grid.length - 1)
                    minMatrix[i][j] = grid[i][j] + minMatrix[i + 1][j];
                else if(j != grid[0].length - 1 && i != grid.length - 1)
                    minMatrix[i][j] = grid[i][j] + Math.min(minMatrix[i + 1][j], minMatrix[i][j + 1]);
                else
                    minMatrix[i][j] = grid[i][j];
            }
        }
        return minMatrix[0][0];
    }
}       
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(mn), as we traverse the entire matrix once.

* **Space Complexity**: O(mn), another matrix of the same size is used.