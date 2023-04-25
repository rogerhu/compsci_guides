## Problem Highlights

* 🔗 **Leetcode Link:** [Island Perimeter](https://leetcode.com/problems/island-perimeter/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Array, 2D-Array, DFS, BFS
* 🗒️ **Similar Questions**: [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/), [Max Area of Island](https://leetcode.com/problems/max-area-of-island/), [Count Sub Islands](https://leetcode.com/problems/count-sub-islands/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input grid be blank??
    - Let’s assume the grid is not blank. We don’t need to consider empty inputs.
- Could there be more than one island?
    - Let's assume there is only one island.
- Is there water inside the island?
    - Let's assume there is no water forming a lake inside the island
- Does the perimeter of the grid count as water or land?
    - The perimeter of the grid counts as water. Out-of-bounds is water.
- What are the time and space constraints?
    - Time complexity should be `O(m*n)`, m being the rows of matrix and n being the columns of matrix. Space complexity should be `O(1)`, excluding the recursive stack.

```markdown
HAPPY CASE
Input: grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
Output: 16
Explanation: The perimeter is the 16 yellow stripes in the image below.
```

![Image 1](https://assets.leetcode.com/uploads/2018/10/12/island.png)

```markdown
Input: grid = [[1,0]]
Output: 4

EDGE CASE

Input: grid = [[1]]
Output: 4
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - A search through the 2D Array (either BFS or DFS) can help us determine the parameter of the island. Which of these two traversals will better help us count the parameter of each piece of land?
- Hash the 2D Array in some way to help with the Strings
    - Hashing would not directly help us find the parameter of the island. However, we could hash where we have counted the parameter for land. 
- Create/Utilize a Trie
    - A Trie would not help us much in this problem since we are not trying to determine anything about a sequence of characters.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Perform a DFS on the island in the grid and recursively we can count every piece of land's perimeter. If it is either a boarder or water return 1. If land was accounted for return 0. If it's unaccounted land, then recursively return perimeter of it's neighbors. 

```markdown
main function:
1) Iterate over the grid
2) If a '1' is seen then "explore" the island from this '1' using dfs and return parameter

dfs function:
1) Basecase 1: for out of bound items, return 1, because the boundary of the square suggest perimeter
2) Basecase 2: for water, return 1, because this side is a perimeter
3) Basecase 3: for '-1' return 0, because we have been there, this is land that we have accounted for
4) Change the grid value to '-1' to mark it as visited and recursively return the perimeter of neighbors to total perimeter of all neighbors.
```

⚠️ **Common Mistakes**
* It can be easy to miss the underlying traversal problem. Make sure to think about how you can optimize traversing the input grid and use the structure to your advantage.
* Some people might forget to use a visited set or some other mechanism to keep track of what coordinates with 1's have already been visited.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        def dfs(i: int, j: int) -> int:
            # Basecase 1: for out of bound items, return 1, because the boundary of the square suggest perimeter
            if i < 0 or i > len(grid)-1 or j < 0 or j > len(grid[i])-1:
                return 1
            # Basecase 2: for water, return 1, because this side is a perimeter
            elif grid[i][j] == 0:
                return 1
            # Basecase 3: for '-1' return 0, because we have been there, this is land that we have accounted for
            elif grid[i][j] == -1:
                return 0
            # Change the grid value to '-1' to mark it as visited and recursively return the perimeter of neighbors to get perimeter of all neighbors.
            else:
                grid[i][j] = -1
                return dfs(i+1, j) + dfs(i-1, j) + dfs(i, j+1) + dfs(i, j-1)
            
        # Iterate over the grid
        for i in range(len(grid)):
            for j in range(len(grid[i])):
                # If a '1' is seen then "explore" the island from this '1' using dfs return perimeter 
                if grid[i][j] == 1:
                    return dfs(i, j)
```
```java
public class Solution {
    public int islandPerimeter(int[][] grid) {
      // Iterate over the grid
      if (grid == null) return 0;
      for (int i = 0 ; i < grid.length ; i++){
        for (int j = 0 ; j < grid[0].length ; j++){
          // If a '1' is seen then "explore" the island from this '1' using dfs return perimeter 
          if (grid[i][j] == 1) {
              return getPerimeter(grid,i,j);
          }
        }
      }
      return 0;
    }
    
    public int getPerimeter(int[][] grid, int i, int j){
      // Basecase 1: for out of bound items, return 1, because the boundary of the square suggest perimeter
      if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length) {return 1;}
      // Basecase 2: for water, return 1, because this side is a perimeter
      if (grid[i][j] == 0) {
          return 1;
      }
      // Basecase 3: for '-1' return 0, because we have been there, this is land that we have accounted for
      if (grid[i][j] == -1) return 0;
      // Change the grid value to '-1' to mark it as visited and recursively return the perimeter of neighbors to get perimeter of all neighbors.
      grid[i][j] = -1;
      return  getPerimeter(grid, i-1, j) + getPerimeter(grid, i, j-1) + getPerimeter(grid, i, j+1) + getPerimeter(grid, i+1, j);
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of rows in 2D-array.
Assume `M` represents the number of columns in 2D-array.

* **Time Complexity**: O(N * M) we need to view each item in the 2D-Array
* **Space Complexity**: O(1) exluding the recursive call stack. The recusive DFS solution will cost us O(N*M) space for the recursive call stack, because we may place the entire grid in the the recursive call stack. 