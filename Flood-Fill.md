## Problem Highlights

* 🔗 **Leetcode Link:** [Flood Fill](https://leetcode.com/problems/flood-fill/)
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: 2D-Array, Breadth-First Search, Depth-First Search
* 🗒️ **Similar Questions**: [Island Perimeter](https://leetcode.com/problems/island-perimeter/), [Max Area of Island](https://leetcode.com/problems/max-area-of-island/), [Coloring A Border](https://leetcode.com/problems/coloring-a-border/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What do we do with 0s?
  - We need to replace any of the 1s on the graph with a 2. However, if there are any other numbers on that graph (such as 0), we should leave them as they are.

- How can we traverse this graph?
  - To traverse through this graph, we can use recursion. We will need some recursive function within our algorithm to repeat until our conditions are met.

- What if the image is null?
  - If the image is null, then we can't do any transformation. Let's return the image if that is the case. If the image is of length 0, we also return the image, as the image is empty.
    


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
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - BFS or DFS would work on this problem. Carrying out DFS is simple on this array by balancing edges cases wherein row and col point to out of index values. After handling those values, we call recursive function again on matrix with the corresponding values of row and col (i.e. [row-1][col], [row+1][col], [row][col-1], [row][col+1]). Now for the algorithm to not recompute on previously computed values, we can use the same check for value of color.
- Hash the 2D Array in some way to help with the Strings
    - We are not working with strings
- Create/Utilize a Trie
    - A Trie will complicate the problem

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use DFS to traverse the matrix and then recursively call function on neighbors above, below, left, and right.
    
0. Check if our starting point color needs to be changed:
    a. If color is as intended then return original image
    b. Otherwise continue.    
1. Store our starting point color in a variable. We are given our starting point through the parameters `image`, `sr`, and `sc`. `sr` represents the row, and `sc` represents the column. 
2. Starts from the starting pixel, changes itself to the new color which is ‘2’ in this case. 
3. Using a dfs call we. can checks its neighbors (left, right, top, bottom), replaces those that had the same number as the one the starting pixel had (which is 1) before it changed to the new color (2). So it changes all the 1s to 2s as long as they are neighbors. Then moves to its left neighbor (1st column) to go through the same process.
    a. Check if neighbor is inbound and is the same as the orginalColor.
        i. if it is out of bound or a different color, then stop
        ii. else repeat.
4. Return the doctored image
   

⚠️ **Common Mistakes**

* A common mistake could include not returning the image at the end so that when the recursion ends at the end of the stack it returns the state of image. Watch out for any errors along the lines of an recursion_depth_exceeded error.
* There is a tricky case where the new color is the same as the original color and if the DFS is done on it, there will be an infinite loop. If new color is same as original color, there is nothing to be done and we can simply return the `image`.

    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        // Check if our starting point color needs to be changed:
        if (image[sr][sc] == color) return image;

        // Otherwise continue. Starts from the starting pixel, changes itself to the new color which is ‘2’ in this case. And run a dfs call to check and change all it's neighbors.
        dfs(image, sr, sc, image[sr][sc], color);

        // return doctored image
        return image;
  }

    // Using a dfs call we. can checks its neighbors (left, right, top, bottom), replaces those that had the same number as the one the starting pixel had (which is 1) before it changed to the new color (2). So it changes all the 1s to 2s as long as they are neighbors. Then moves to its left neighbor (1st column) to go through the same process.
    private void dfs(int[][] image, int sr, int sc, int originalColor, int newColor) {
        // Check if neighbor is inbound and is the same as the orginalColor.
        // if it is out of bound or a different color, then stop
        if (sr < 0 || sr >= image.length || sc < 0 || sc >= image[0].length || image[sr][sc] != originalColor) return;

        // else repeat.
        image[sr][sc] = newColor;
        dfs(image, sr + 1, sc, originalColor, newColor);
        dfs(image, sr - 1, sc, originalColor, newColor);
        dfs(image, sr, sc + 1, originalColor, newColor);
        dfs(image, sr, sc - 1, originalColor, newColor);
    }
}
```
    
```python
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        # Check if our starting point color needs to be changed: If color is as intended then return original image. Otherwise continue.    
        if (image[sr][sc] == color):
            return image

        # Store our starting point color in a variable. We are given our starting point through the parameters `image`, `sr`, and `sc`. `sr` represents the row, and `sc` represents the column. 
        m, n, originalColor = len(image), len(image[0]), image[sr][sc]

        # Using a dfs call we. can checks its neighbors (left, right, top, bottom), replaces those that had the same number as the one the starting pixel had (which is 1) before it changed to the new color (2). So it changes all the 1s to 2s as long as they are neighbors. Then moves to its left neighbor (1st column) to go through the same process.
        def dfs(i,j):
            # Check if neighbor is inbound and is the same as the orginalColor.
            if i < 0 or i > m-1 or j < 0 or j > n-1 or image[i][j] != originalColor:
                # if it is out of bound or a different color, then stop
                return
            
            # else repeat.
            image[i][j] = color
            dfs(i+1, j)
            dfs(i-1, j)
            dfs(i, j+1)
            dfs(i, j-1)

        # Starts from the middle as the starting pixel, changes itself to the new color which is ‘2’ in this case. And run a dfs call to check and change all it's neighbors.
        dfs(sr, sc)
        
        # Return the doctored image
        return image```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* Time Complexity: `O(N)`, where N is the number of pixels in the image. We might process every pixel.
* Space Complexity: `O(N)`, the size of the implicit call stack when calling DFS.