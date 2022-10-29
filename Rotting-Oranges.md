## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/rotting-oranges/](https://leetcode.com/problems/rotting-oranges/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search
* 🗒️ **Similar Questions**: TBD

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

## 1: **U-nderstand**
    
- What do the possible values of the grid represent? 
  - 1’s are fresh oranges, 2’s are rotten oranges and 0’s are empty spaces
    
- What data structures can I use to store the grids? 
  - You can use a 2D array, hashset, queue, etc.
    
- Do we need to keep track of the level? 
  - Yes, you can keep track of the level using a search algorithm. Trick is to only increment once per level and only if fresh.
    
- What is a possible edge case? That there is no fresh, there is no rotten
    
```markdown
    HAPPY CASE
    Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
    Output: 4
    
    Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
    Output: -1
    
    EDGE CASE
    Input: grid = [[0,2]]
    Output: 0
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
- BFS: We can model the grid in the form of a graph and to compute the distance for every node, we can apply standard BFS algorithm using a queue. We can think of it in a level by level manner. The key observation is that fresh oranges adjacent to rotten oranges are rotten on day 1, those adjacent to those oranges are rotten on day 2, and so on. The phenomenon is similar to a level order traversal on a graph, where all the initial rotten oranges act as root nodes. We can just push all these root nodes into a queue and perform BFS on a grid algorithm, to calculate the total time taken to rot all the oranges. Since there can be multiple rotten cells, we will push all those cells in the queue first and then continue with the BFS. If all oranges are not rotten before our algorithm terminates, we will return -1. 
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
    1. Initialize a queue for breadth first search.
    2. Iterate over the entire grid and add all the rotten oranges in the queue and also keep counting the number of fresh oranges.
    3. If the number of fresh oranges is zero then we can directly return zero.
    4. Otherwise, traverse the queue in level order fashion and add all the adjacent fresh oranges in the queue and decrement the count of fresh oranges by 1 each time. When we add a fresh orange in the queue, we mark it as rotten so that it is not added multiple times.
    5. If after one complete traversal of a level, the queue is not empty, then increase the minutes by one.
    6. Repeat this process until we have no more rotten oranges.
    7. If the number of fresh oranges after the entire process is still not zero, then return -1 indicating that it’s impossible to rot all the oranges.
    8. Else return the time required to rot all the oranges.


⚠️ **Common Mistakes**

* 

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    static int dx[] = {1, 0, -1, 0};
    static int dy[] = {0, 1, 0, -1};
    public static int numberOfDays(int[][] grid) {
      int r = grid.length;
      int c = grid[0].length;
      int days = 0, countOfOnes = 0;
      Queue < int[] > q = new LinkedList < > ();
      for (int i = 0; i < r; i++) {
        for (int j = 0; j < c; j++) {
          if (grid[i][j] == 2) {
            q.add(new int[] {i, j});
          }
          if (grid[i][j] == 1) {
            countOfOnes += 1;
          }
        }
      }
      if (countOfOnes == 0) {
        return 0;
      }
      while (!q.isEmpty()) {
        int size = q.size();
        while (size--> 0) {
          int[] temp = q.remove();
          int i = temp[0], j = temp[1];
          for (int l = 0; l < 4; l++) {
            int nr = i + dx[l], nc = j + dy[l];
            if (nr < 0 || nc < 0 || nr == r || nc == c) {
              continue;
            }
            if (grid[nr][nc] == 1) {
              countOfOnes -= 1;
              q.add(new int[] {
                nr,nc
              });
              grid[nr][nc] = 2;
            }
          }
        }
        if (q.size() > 0) {
          days += 1;
        }
      }
      return countOfOnes == 0 ? days : -1;
    }
```
    
```python
    def numberOfDays(grid):
        r = len(grid)
        c = len(grid[0])
        fresh = 0
        queue = []
        vis = set()
        for i in range(r):
            for j in range(c):
                if grid[i][j] == 2:
                    queue.append((i, j, 0))
                    vis.add((i, j))
                elif grid[i][j] == 1:
                    fresh += 1
        if not fresh:
            return 0
        while queue:
            size = len(queue)
    
            while size:
                x, y, days = queue.pop(0)
                if x < r - 1 and (x + 1, y) not in vis and grid[x + 1][y] == 1:
                    queue.append((x + 1, y, days + 1))
                    vis.add((x + 1, y))
                    fresh -= 1
                if x > 0 and (x - 1, y) not in vis and grid[x - 1][y] == 1:
                    queue.append((x - 1, y, days + 1))
                    vis.add((x - 1, y))
                    fresh -= 1
                if y < c - 1 and (x, y + 1) not in vis and grid[x][y + 1] == 1:
      queue.append((x, y + 1, days + 1))
                    vis.add((x, y + 1))
                    fresh -= 1
                if y > 0 and (x, y - 1) not in vis and grid[x][y - 1] == 1:
                    queue.append((x, y - 1, days + 1))
                    vis.add((x, y - 1))
                    fresh -= 1
                size -= 1
        return -1 if fresh else days
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(n*m)`, where n — number of rows m– number of cols, for first traversal of the grid to find all the rotten and fresh oranges + O(n*m) for queue traversal if there is only one fresh orange and that too when the last orange is fresh.
<br>
Space Complexity: `O(n*m)`, where n — number of rows m– number of cols, extra space for queue in the worst case when all the oranges are rotten.