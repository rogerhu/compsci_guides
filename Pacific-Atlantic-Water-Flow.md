## Problem Highlights

* 🔗 **Leetcode Link:** [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, 2D-Array, DFS, BFS
* 🗒️ **Similar Questions**: [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/), [Number of Islands](https://leetcode.com/problems/number-of-islands/), [Count Sub Islands](https://leetcode.com/problems/count-sub-islands/),[Max Area of Island](https://leetcode.com/problems/max-area-of-island/) 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input grid be blank?
    - Let’s assume the grid is not blank. We don’t need to consider empty inputs.
- What is the time and space constraints?
    - Time complexity should be `O(m*n)`, m being the rows of the matrix and n being the columns of matrix. Space complexity should be `O(m*n)`.


```markdown
HAPPY CASE
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans..
```

![Image1](https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg)

```markdown
Input: grid = [[0,2,2,0]]
Output: [[0,0],[0,1],[0,2][0,3]]

EDGE CASE
Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - A search through the 2D Array (either BFS or DFS) can help us find all items that touch the pacific ocean and atlantic ocean.
- Hash the 2D Array in some way to help with the Strings
    - Hashing would not directly help us find islands. However, hashing will help us keep track of the islands we have visited.   
- Create/Utilize a Trie
    - A Trie would not help us much in this problem since we are not trying to determine anything about a sequence of characters.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Lets gather all spaces that touches the pacific ocean and all spaces that touches the atlantic ocean, the union of both would result in spaces that touches both the pacific ocean and atlantic ocean.

```markdown
main function:
0) Write the helper dfs function
1) Initialize a variable to keep track of the coordinates that touch the pacific or atlantic
2) Iterate over the top row and bottom row 
3) Iterate over the left most column and right most column
4) Return the union of touch pacific and atlantic

helper dfs function:
0) Basecase: Out of bound or space has been added to touchesOcean, return 
1) If the coordinate has a height that is equal or greater than previous maxHeight, then it touches ocean
    a) Add this coordinate to the touchesOcean set
    b) Check the other for neighbors from this coordinate
```

⚠️ **Common Mistakes**
* It's easy to brute force a solution by checking each item if it can get to both the pacific ocean and atlantic ocean, but we must consider how we can break the problem down. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        def dfs(i, j, touchesOcean, maxHeight):
            # Basecase: Out of bound or space has been added to touchesOcean, return 
            if i < 0 or i >= len(heights) or j < 0 or j >= len(heights[0]) or (i,j) in touchesOcean:
                return
            
            # If the coordinate has a height that is equal or greater than previous maxHeight, then it touches ocean
            elif heights[i][j] >= maxHeight:

                # Add this coordinate to the touchesOcean set
                touchesOcean.add((i,j))

                # Check the other for neighbors from this coordinate
                dfs(i-1,j,touchesOcean,heights[i][j])
                dfs(i+1,j,touchesOcean,heights[i][j])
                dfs(i,j-1,touchesOcean,heights[i][j])
                dfs(i,j+1,touchesOcean,heights[i][j])

        # Initialize a variable to keep track of the coordinates that touch the pacific or atlantic
        touchesPacific, touchesAtlantic = set(), set()

        # Iterate over the top row and bottom row 
        for j in range(len(heights[0])):
            dfs(0,j, touchesPacific, 0)
            dfs(len(heights)-1,j, touchesAtlantic, 0)
        
        # Iterate over the left most column and right most column
        for i in range(len(heights)):
            dfs(i,0, touchesPacific, 0)
            dfs(i,len(heights[0])-1, touchesAtlantic, 0)
        
        # Return the union of touch pacific and atlantic
        return list(touchesPacific & touchesAtlantic)
```
```java
class Solution {
    public List<List<Integer>> pacificAtlantic(int[][] heights) {
      // Initialize a variable to keep track of the coordinates that touch the pacific or atlantic
      int rows = heights.length, cols = heights[0].length;
      boolean[][] pac = new boolean[rows][cols];
      boolean[][] atl = new boolean[rows][cols];
        
      // Iterate over the top row and bottom row 
      for (int col = 0; col< cols; col++){
          dfs(0, col, rows, cols, pac, heights[0][col], heights);
          dfs(rows-1, col,rows, cols, atl, heights[rows-1][col], heights);
      }

      // Iterate over the left most column and right most column
      for (int row = 0; row<rows; row++){
          dfs(row, 0,rows, cols, pac, heights[row][0], heights);
          dfs(row, cols-1,rows, cols, atl, heights[row][cols-1], heights);
      }

      // Return the union of touch pacific and atlantic
      List<List<Integer>> result = new ArrayList<List<Integer>>();
      for (int i = 0; i < rows; i++)
          for (int j = 0; j < cols; j++){
              if (pac[i][j] && atl[i][j])
                  result.add(Arrays.asList(i,j));
          }
      return result;
    }
    
    private void dfs(int row, int col, int rows, int cols, boolean[][] visited, int prevHeight, int[][] heights){
      //  Basecase: Out of bound or space has been added to touchesOcean, return 
      // If the coordinate has a height that is equal or greater than previous maxHeight, then it touches ocean
      if (row < 0 || row >= rows || col < 0 || col >= cols || visited[row][col] || prevHeight > heights[row][col])
          return;
      // Add this coordinate to the touchesOcean set
      visited[row][col]= true;

      // Check the other for neighbors from this coordinate
      dfs(row+1, col, rows, cols, visited, heights[row][col], heights);
      dfs(row-1, col, rows, cols, visited, heights[row][col], heights);
      dfs(row, col+1, rows, cols, visited, heights[row][col], heights);
      dfs(row, col-1, rows, cols, visited, heights[row][col], heights);
        
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
Assume`M` represents the number of columns in 2D-array.


* **Time Complexity**: `O(N * M)` we may need to view each item in the 2D-Array
* **Space Complexity**: `O(N * M)`, the cost of storing the touches Pacific and touches Atlantic sets may contain all the items in the 2D-Array.