🔗 **Leetcode Link:** [https://leetcode.com/problems/flood-fill/](https://leetcode.com/problems/flood-fill/)

⏰ **Time to complete**: 17 mins

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

1. **U-nderstand**

- What do we do with 0s?
We need to replace any of the 1s on the graph with a 2. However, if there are any other numbers on that graph (such as 0), we should leave them as they are.

- How can we traverse this graph?
To traverse through this graph, we can use recursion. We will need some recursive function within our algorithm to repeat until our conditions are met.

- What if the image is null?
If the image is null, then we can't do any transformation. Let's return the image if that is the case. If the image is of length 0, we also return the image, as the image is empty.
    
    ```markdown
    HAPPY CASE
    Input: image = [[1,1,1],[1,1,0],[1,0,1]], sr = 1, sc = 1, color = 2
    Output: [[2,2,2],[2,2,0],[2,0,1]]
    
    Input: image = [[0,0,0],[0,0,0]], sr = 0, sc = 0, color = 0
    Output: [[0,0,0],[0,0,0]]
    
    EDGE CASE
    Input: [[1,0,0],[0,1,0],[0,0,1]], sr = 0, sc = 0, color = 0
    Output: [[0,0,0],[0,1,0],[0,0,1]]
    ```
    
2. M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
    - BFS or DFS would work on this problem. Carrying out DFS is simple on this array by balancing edges cases wherein row and col point to out of index values. After handling those values, we call recursive function again on matrix with the corrsponding values of row and col (i.e. [row-1][col], [row+1][col], [row][col-1], [row][col+1]). Now for the algorithm to not recompute on previously computed values, we can use the same check for value of color.


3. P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
    Store our starting point in a variable. We are given our starting point through the parameters `image`, `sr`, and `sc`. `sr` represents the row, and `sc` represents the column. Step 1 starts from the middle as the starting pixel, changes itself to the new color which is ‘2’ in this case. It checks its neighbors (left, right, top, bottom), replaces those that had the same number as the one the starting pixel had (which is 1) before it changed to the new color (2). So it changes all the 1s to 2s as long as they are neighbors. Then moves to its left neighbor (1st column) to go through the same process.
    
    Step 2, changes its top and bottom neighbors to 2 because those neighbors had the same number as its initial before itself was changed. 
    
    Step 3 and 4 go through the same process.
    
    There is a tricky case where the new color is the same as the original color and if the DFS is done on it, there will be an infinite loop. If new color is same as original color, there is nothing to be done and we can simply return the `image`.
    
4. I-mplement

> **Implement** the code to solve the algorithm.
    
    ```java
    class Solution {
      public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
    	   if (image[sr][sc] == newColor) return image;
    	   fill(image, sr, sc, image[sr][sc], newColor);
    	   return image;
    	}
    
    	private void fill(int[][] image, int sr, int sc, int color, int newColor) {
    	   if (sr < 0 || sr >= image.length || sc < 0 || sc >= image[0].length || image[sr][sc] != color) return;
    	   image[sr][sc] = newColor;
    	   fill(image, sr + 1, sc, color, newColor);
    	   fill(image, sr - 1, sc, color, newColor);
    	   fill(image, sr, sc + 1, color, newColor);
    	   fill(image, sr, sc - 1, color, newColor);
    	}
    }
    ```
    
    ```python
    class Solution:
        def floodFill(self, image, sr, sc, newColor):
    	   if (image[sr][sc] == newColor):
    	       return image
    	   m, n, originalColor = len(image), len(image[0]), 
    	image[sr][sc]
    	   def dfs(i,j):
    	       if i < 0 or i > m-1 or j < 0 or j > n-1 or 
    	image[i][j] != originalColor:
    	           return
    	       image[i][j] = newColor
    	       dfs(i+1, j)
    	       dfs(i-1, j)
    	       dfs(i, j+1)
    	       dfs(i, j-1)
    	   dfs(sr, sc)
    	   return image
    ```
    
5. R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
6. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

    - Time Complexity: `O(NM)`, note: every element of matrix is processed
    - Space Complexity: `O(NM)`