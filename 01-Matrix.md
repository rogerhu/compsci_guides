## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/01-matrix/](https://leetcode.com/problems/01-matrix/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search
* 🗒️ **Similar Questions**: [Shortest Path to Get Food](https://leetcode.com/problems/shortest-path-to-get-food/), [Minimum Operations to Remove Adjacent Ones in Matrix](https://leetcode.com/problems/minimum-operations-to-remove-adjacent-ones-in-matrix/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we approach a neighboring cell? 
  - When we find the neighbor, we just ignore the visited position, this will lead you to find the new neighbor, and exactly level by level.
    
- What should we keep track of when we visit a cell? 
  - Every time you visit a node, it will be from a path of predecessors that is of shortest distance to a zero.
    
- What coordinates should we store?
  - Store all the coordinates which have 1s.
    


```markdown
    HAPPY CASE
    Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
    Output: [[0,0,0],[0,1,0],[0,0,0]]
    
    Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
    Output: [[0,0,0],[0,1,0],[1,2,1]]

    EDGE CASE
    Input: mat = [[0,0,0],[0,0,0],[0,0,0]]
    Output: [[0,0,0],[0,0,0],[0,0,0]]
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- BFS: The idea is to put all zeroes in one BFS layer/breadth and move out to other nodes from there. Mark unvisited node as `-1`. The first `matrix[vrow][vcol] = matrix[urow][ucol] + 1`should be the smallest distance possible.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- DFS: We can use DFS to traverse the graph until we find a valid itinerary, ensuring we choose the lexicographically least itinerary.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
1. apply bfs on zero values and store -1 for other matrix data to denote they are not visited yet.
2. traverse level order wise and for each level update distance only of those indexes who has -1 assigned and currently neighbor of queue.poll.
3. add such element to back of queue also for next level traversal.
4. in this way those who are not reachable to any zero in first attempt (i.e.
first level), for them new level is checked and hence length counter will increased by 1
5. now for those new queue element length will be set to updated length if
that cell has -1.

⚠️ **Common Mistakes**

* How do you avoid having the recursion relation going on forever? We can start sweeps from top right and bottom left. With min(up, left) first, then this part of recursion relation ends at bottom right. After that, min(right, down) in the second sweep to end the recursion relation at top left. 
    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        public int[][] updateMatrix(int[][] mat) {
    		// Queue to hold the co-ordinates
    		Queue<int[]> queue = new LinkedList<>();
    		for (int i = 0; i < mat.length; i++) {
    			for (int j = 0; j < mat[i].length; j++) {
    				if (mat[i][j] == 0) {
    					// only those added to queue who has 0 value.
    					queue.add(new int[] { i, j });
    				} else {
    					// else set value to -1 to indicate length needed to be updated here.
    					mat[i][j] = -1;
    				}
    			}
    		}
    		int[][] dirs = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };
    		int length = 0;
    		while (!queue.isEmpty()) {
    			int size = queue.size();
    			// hold level count
    			length++;
    			// traverse level order wise
    			while (size-- > 0) {
    				int[] rc = queue.poll();
    				// for each element in queue try all 4 directions
    				for (int[] dir : dirs) {
    					int r = rc[0] + dir[0];
    					int c = rc[1] + dir[1];
    					// if out of range or -1 it means no need to process it.
    					if (r < 0 || c < 0 || r >= mat.length || c >= mat[0].length || mat[r][c] != -1) {
    						continue;
    					}
    					// if not already -1. update length
    					mat[r][c] = length;
    					// add element to queue for processing
    					queue.add(new int[] { r, c });
    				}
    			}
    		}
    
    		return mat;
    	}
    }
```
    
```python
def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
    num_rows, num_cols = len(mat), len(mat[0])
	queue = deque()
	inf_value = float('inf')

	for i in range(num_rows):
		for j in range(num_cols):
			if mat[i][j] == 0:
				queue.append((i, j))
			else:
				mat[i][j] = inf_value

	# BFS from each zero cell rather than just one cell
	while queue:
		i, j = queue.popleft()    

		# Set the value if it has not been visited (value is inf)
		# Don't need to set if value is actually a zero (or even some number)
		if mat[i][j] == inf_value:
			up = down = left = right = inf_value
			if i-1 >= 0:
				up = mat[i-1][j] 
			if i+1 < num_rows:
				down = mat[i+1][j]
			if j-1 >= 0:
				left = mat[i][j-1]
			if j+1 < num_cols:
				right = mat[i][j+1]

			min_value = min(down, up, left, right)
			mat[i][j] = min_value + 1

		# Add any neighbours to the queue
		if i-1 >= 0 and mat[i-1][j] == inf_value:
			queue.append((i-1, j))
		if i+1 < num_rows and mat[i+1][j] == inf_value:
			queue.append((i+1, j))
		if j-1 >= 0 and mat[i][j-1] == inf_value:
			queue.append((i, j-1))
		if j+1 < num_cols and mat[i][j+1] == inf_value:
			queue.append((i, j+1))

	return mat
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time complexity: O(r⋅c)
Since, the new cells are added to the queue only if their current distance is greater than the calculated distance, cells are not likely to be added multiple times.
<br>
Space complexity: O(r⋅c), where an additional O(r⋅c) space is required to maintain the queue.