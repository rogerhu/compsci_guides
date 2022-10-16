🔗 **Leetcode Link:** [https://leetcode.com/problems/01-matrix/](https://leetcode.com/problems/01-matrix/)

⏰ **Time to complete**:13 mins

1. **U-nderstand**
    1. How do we approach a neighboring cell? 
    
    When we find the neighbor, we just ignore the visited position, this will lead you to find the new neighbor, and exactly level by level.
    
    b. What should we keep track of when we visit a cell? 
    
    Every time you visit a node, it will be from a path of predecessors that is of shortest distance to a zero.
    
    c. What coordinates should we store?
    
    Store all the coordinates which have 1s.
    
    ```markdown
    HAPPY CASE
    Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
    Output: [[0,0,0],[0,1,0],[0,0,0]]
    
    Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
    Output: [[0,0,0],[0,1,0],[1,2,1]]
    
    ```
    
2. M-atch
    - BFS: The idea is to put all zeroes in one BFS layer/breadth and move out to other nodes from there. Mark unvisited node as `-1`. The first `matrix[vrow][vcol] = matrix[urow][ucol] + 1`should be the smallest distance possible.
3. P-lan
    
    General Description of plan (1-2 sentences)
    
    1. apply bfs on zero values and store -1 for other matrix data to denote they are not visited yet.
    2. traverse level order wise and for each level update distance only of those
    indexes who has -1 assigned and currently neighbor of queue.poll.
    3. add such element to back of queue also for next level traversal.
    4. in this way those who are not reachable to any zero in first attempt (i.e.
    first level), for them new level is checked and hence length counter will increased by 1
    5. now for those new queue element length will be set to updated length if
    that cell has -1.
    
4. I-mplement
    
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
    from collections import deque
    
    class Solution:
        def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
            rows, cols = len(mat), len(mat[0])
            seen = set()
            q = deque()
            
            for r in range(rows):
                for c in range(cols):
                    if mat[r][c] == 0:
                        q.append((r, c))
                        seen.add((r, c))
            
            coords = [(0,1), (1,0), (0,-1), (-1,0)]
            distance = 1
            while q:
                # for distance += 1 at the end of the level traversal.
                for _ in range(len(q)):
                    row, col = q.popleft()
                    for rc, cc in coords:
                        r = row + rc
                        c = col + cc
    
                        if r >= 0 and r < rows and c >= 0 and c < cols and (r, c) not in seen:
                            mat[r][c] = distance
                            q.append((r, c))
                            seen.add((r, c))
                        
                distance += 1
            return mat
    ```
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: **O(m*n)**
    - Space Complexity: **O(m*n)**