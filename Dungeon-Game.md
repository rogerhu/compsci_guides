## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/dungeon-game/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Minimum Path Cost in a Grid](https://leetcode.com/problems/minimum-path-cost-in-a-grid/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Why does the knight need to go RIGHT->RIGHT->DOWN->DOWN path? What if he went DOWN->DOWN->RIGHT-RIGHT path, there is lot of positive energy in this path. He doesn't need initial energy to start with, and he could gather positive energy on the way.
   - If the knight follows the DOWN->DOWN->RIGHT-RIGHT path, the knight would need 8 health to begin with to not die when hitting the left bottom corner.

- What is the minimum initial health required for the knight to survive the journey (aka. bare minimum health points)?
   - The bare minimum health points corresponds with the healthiest path (aka. most energy preserving path)

- How can we achieve the healthiest path?
   - To achive the healthiest path, the knight needs to constantly choose: the cell that consumes the least energy of all available cells (aka. the cell with the least powerful demon) or even better, choose cells that provides the knight with more energy (aka. cells containing magical orbs).


   
```markdown
Example 1:
Input: dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]
Output: 7
Explanation: The initial health of the knight must be at least 7 if he follows the optimal path: RIGHT-> RIGHT -> DOWN -> DOWN.

Example 2:
Input: dungeon = [[0]]
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

As a general pattern of dynamic programming, usually we construct a array of one or two dimensions (i.e. dp[i]) where each element holds the optimal solution for the corresponding subproblem.

To calculate one particular element in the dp[i] array, we would refer to the previously calculated elements. And the last element that we figure out in the array would be the desired solution for the original problem. We need to examine the optimal path and look at our knight's health at each step along the journey. The lowest health points is the worst health condition that our knight had to endure during the journey. That value should be the basis upon which the minimum initial health is to be calculated.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** The main idea is to build a 2D array dp to store the minimum initial health required at each position to reach the bottom-right corner. We start from the bottom-right corner and work our way backwards to the top-left corner.

```markdown
At each position, we take the minimum of the minimum initial health required at the position below and the position to the right, and subtract the dungeon value at the current position. If the result is less than or equal to 0, we set it to 1 (since the knight needs to have at least 1 health point to be alive). Finally, we return the value at the top-left corner of the dp array.

To handle the boundary cases, we add an extra row and an extra column to the dp array, and set them all to infinity except for dp[m-1][n] and dp[m][n-1], which are set to 1.

```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?


At [i,j], you can take a step right or down. Let us call the cost from these points to the destination as right and down. Remember cost is the least cumulative health. Between the two, you will pick a path which is larger of right or down since that path would require minimal initial health. For example, say right was -6 and down was -7, then we will choose right since that minimizes the health needed.
Now what about the health of the new augmented path which should include grid[i.j]. With addition of this co-ordinate, the new cumulative cost is: max(right, down) + grid[i,j]
However, grid[i,j] might be negative or less than max(right, down) + grid[i,j]. Therefore this will then define the total cost of this path. We thus have recurrence: cost[i,j] = min(max(right, down) + grid[i, j], grid[i,j])

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def calculateMinimumHP(self, dungeon: List[List[int]]) -> int:
        m, n = len(dungeon), len(dungeon[0])
        dp = [[float('inf')] * (n+1) for _ in range(m+1)]
        dp[m-1][n] = dp[m][n-1] = 1
        for i in range(m-1, -1, -1):
            for j in range(n-1, -1, -1):
                dp[i][j] = max(min(dp[i+1][j], dp[i][j+1]) - dungeon[i][j], 1)
        return dp[0][0]
```
```java
class Solution {
  int inf = Integer.MAX_VALUE;
  int[][] dp;
  int rows, cols;

  public int getMinHealth(int currCell, int nextRow, int nextCol) {
    if (nextRow >= this.rows || nextCol >= this.cols)
      return inf;
    int nextCell = this.dp[nextRow][nextCol];
    // hero needs at least 1 point to survive
    return Math.max(1, nextCell - currCell);
  }

  public int calculateMinimumHP(int[][] dungeon) {
    this.rows = dungeon.length;
    this.cols = dungeon[0].length;
    this.dp = new int[rows][cols];
    for (int[] arr : this.dp) {
      Arrays.fill(arr, this.inf);
    }
    int currCell, rightHealth, downHealth, nextHealth, minHealth;
    for (int row = this.rows - 1; row >= 0; --row) {
      for (int col = this.cols - 1; col >= 0; --col) {
        currCell = dungeon[row][col];

        rightHealth = getMinHealth(currCell, row, col + 1);
        downHealth = getMinHealth(currCell, row + 1, col);
        nextHealth = Math.min(rightHealth, downHealth);

        if (nextHealth != inf) {
          minHealth = nextHealth;
        } else {
          minHealth = currCell >= 0 ? 1 : 1 - currCell;
        }
        this.dp[row][col] = minHealth;
      }
    }
    return this.dp[0][0];
  }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(mn)
* **Space Complexity**: O(mn)