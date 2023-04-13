## Problem Highlights

* 🔗 **Leetcode Link:** [01 Matrix](https://leetcode.com/problems/01-matrix/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, 2D-Array, DFS, BFS
* 🗒️ **Similar Questions**: [Difference Between Ones and Zeros in Row and Column](https://leetcode.com/problems/difference-between-ones-and-zeros-in-row-and-column/), [Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we approach a neighboring cell?
    - When we find the neighbor, we just ignore the visited position, this will lead you to find the new neighbor, and exactly level by level.
- What should we keep track of when we visit a cell?
    - Every time you visit a node, it will be from a path of predecessors that is of the shortest distance to a zero.


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
    - BFS: The idea is to put all zeroes in one BFS layer/breadth and move out to other nodes from there. 
- Hash the 2D Array in some way to help with the Strings
    - We are not working with strings
- Create/Utilize a Trie
    - A Trie will complicate the problem

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Apply BFS to queue up all zero node and look at the next layers from zero as we repeat the bfs call to signify the distance of each position. 

```python
PYTHON
1) Queue up all the zero values 
2) Traverse level order wise and for each level and update distance
    a) Add such element to back of queue also for next level traversal. In this way those who are not reachable to any zero in first attempt (i.e.first level), the new level is checked and hence length counter will increased by 1
```
```java
JAVA
1. apply bfs on zero values and store -1 for other matrix data to denote they are not visited yet.
2. traverse level order wise and for each level update distance only of those
indexes who has -1 assigned and currently neighbor of queue.poll.
3. add such element to back of queue also for next level traversal.
4. in this way those who are not reachable to any zero in first attempt (i.e.
first level), for them new level is checked and hence length counter will increased by 1
5. now for those new queue element length will be set to updated length if
that cell has -1.
```

⚠️ **Common Mistakes**
* How do you avoid having the recursion relation going on forever? We can start sweeps from top right and bottom left. With min(up, left) first, then this part of recursion relation ends at bottom right. After that, min(right, down) in the second sweep to end the recursion relation at top left.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        rows, cols = len(mat), len(mat[0])
        seen = set()
        q = deque()

        # Queue up all the zero values 
        for r in range(rows):
            for c in range(cols):
                if mat[r][c] == 0:
                    q.append((r, c))
                    seen.add((r, c))

    # Traverse level order wise and for each level and update distance
        coords = [(0,1), (1,0), (0,-1), (-1,0)]
        distance = 1
        while q:
            for _ in range(len(q)):
                row, col = q.popleft()
                for rc, cc in coords:
                    r = row + rc
                    c = col + cc
                    # Add such element to back of queue also for next level traversal. In this way those who are not reachable to any zero in first attempt (i.e.first level), the new level is checked and hence length counter will increased by 1
                    if r >= 0 and r < rows and c >= 0 and c < cols and (r, c) not in seen:
                        mat[r][c] = distance
                        q.append((r, c))
                        seen.add((r, c))

            distance += 1
        return mat
```

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
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of rows in 2D-array.
Assume`M` represents the number of columns in 2D-array.


* **Time Complexity**: O(N * M) we need to view each item in the 2D-Array
* **Space Complexity**: O(N * M) we may need to store N * M items in the queue at one time.  