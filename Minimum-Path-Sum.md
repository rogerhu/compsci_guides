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

- 
   
```markdown
HAPPY CASE


EDGE CASE

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1. We first create an m by n minMatrix. This matrix represents the minimum distance to travel starting from square (i, j) to get to corner element (m-1, n-1). At the end of this algorithm we really are looking for (0,0). Pause and think about this.
2. We recursively call minPathSum to populate minMatrix starting from (0,0). Note, this will fully populate minMatrix.
3, We check bounds at the top of the recursion OR if the grid element located at (i,j) has been visited in the past. Note, we leverage the fact that the grid has ONLY non-negative integers here, so during recursion we can use -1 as a flag to indicate if it has been visited.
4. We check if minMatrix already contains a non-negative entry at (i,j). This leverages the fact that if this is populated then it must have already gone through the recursion to identify this value. Note, this is generaly refered to as memoization.
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
       
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: 
* **Space Complexity**: 