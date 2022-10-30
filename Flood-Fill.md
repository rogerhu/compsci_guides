## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/flood-fill/](https://leetcode.com/problems/flood-fill/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search, Depth-First Search
* 🗒️ **Similar Questions**: [Island Perimeter](https://leetcode.com/problems/island-perimeter/)

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
    
For graph problems, some things we want to consider are:
    
- BFS/DFS: BFS or DFS would work on this problem. Carrying out DFS is simple on this array by balancing edges cases wherein row and col point to out of index values. After handling those values, we call recursive function again on matrix with the corresponding values of row and col (i.e. [row-1][col], [row+1][col], [row][col-1], [row][col+1]). Now for the algorithm to not recompute on previously computed values, we can use the same check for value of color.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.


## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
1. Store our starting point in a variable. We are given our starting point through the parameters `image`, `sr`, and `sc`. `sr` represents the row, and `sc` represents the column. Step 1 starts from the middle as the starting pixel, changes itself to the new color which is ‘2’ in this case. It checks its neighbors (left, right, top, bottom), replaces those that had the same number as the one the starting pixel had (which is 1) before it changed to the new color (2). So it changes all the 1s to 2s as long as they are neighbors. Then moves to its left neighbor (1st column) to go through the same process.
2. Change top and bottom neighbors to 2 because those neighbors had the same number as its initial before itself was changed. 
Step 3 and 4 go through the same process.
   

⚠️ **Common Mistakes**

* A common mistake could include not returning the image at the end so that when the recursion ends at the end of the stack it returns the state of image. Watch out for any errors along the lines of an recursion_depth_exceeded error.
* There is a tricky case where the new color is the same as the original color and if the DFS is done on it, there will be an infinite loop. If new color is same as original color, there is nothing to be done and we can simply return the `image`.

    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        int previousColor=image[sr][sc];
        fillColor(image,sr,sc,newColor,previousColor);
        return image;
    }
    
    public void fillColor(int[][] image, int sr, int sc, int newColor,int previousColor){
        //change sr sc index color to new color
        image[sr][sc]=newColor;
        
       // check for up and change the color to new Color if up is not visited yet
        if(sr-1>=0 && image[sr-1][sc]==previousColor && image[sr-1][sc]!=newColor){
            fillColor(image,sr-1,sc,newColor,previousColor);
        }
        // check for down and change the color to new Color if down is not visited yet
        if(sr+1<image.length && image[sr+1][sc]==previousColor && image[sr+1][sc]!=newColor){
            fillColor(image,sr+1,sc,newColor,previousColor);
        }
        // check for left and change the color to new Color if left is not visited yet
        if(sc-1>=0 && image[sr][sc-1]==previousColor && image[sr][sc-1]!=newColor){
            fillColor(image,sr,sc-1,newColor,previousColor);
        }
        // check for right and change the color to new Color if right is not visited yet
        if(sc+1<image[0].length && image[sr][sc+1]==previousColor && image[sr][sc+1]!=newColor){
            fillColor(image,sr,sc+1,newColor,previousColor);
        }
    }
}
```
    
```python
import queue
class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
        # BFS
        m, n = len(image), len(image[0])
        visited = [['w' for i in range(n)] for j in range(m)] # white as not-yet discovered/visited
        Q = queue.Queue()
        srcColor = image[sr][sc] 
        image[sr][sc] = newColor
        Q.put((sr, sc))
        visited[sr][sc] = 'g'
        while not Q.empty():
            (i,j) = Q.get()
            # adjacent nodes to traverse 
            adjacents = [(i-1, j), (i+1, j), (i, j-1), (i,j+1)]
            for node in adjacents:
                (x,y) = node
                # check if the asjacent node are of legal indices 
                if (0<=x and x<m) and (0<=y and y < n):
                    # check if node is not yet discovered and needs to fill flood
                    if (image[x][y] == srcColor) and (visited[x][y] == 'w'):
                        image[x][y]= newColor
                        visited[x][y] = 'g'
						# push the adjacent node into Queue (mark gray as discovered but needs to be visited)
                        Q.put((x,y)) 
			# mark visited (black)
            visited[i][j] = 'b'
        return image
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* Time Complexity: `O(N)`, where N is the number of pixels in the image. We might process every pixel.
* Space Complexity: `O(N)`, the size of the implicit call stack when calling DFS.