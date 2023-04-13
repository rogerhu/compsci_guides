## Problem Highlights

* 🔗 **Leetcode Link:** [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, 2D-Array, BFS
* 🗒️ **Similar Questions**: [Detonate the Maximum Bombs](https://leetcode.com/problems/detonate-the-maximum-bombs/), [Escape the Spreading Fire](https://leetcode.com/problems/escape-the-spreading-fire/), [Last Day Where You Can Still Cross](https://leetcode.com/problems/last-day-where-you-can-still-cross/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What do the possible values of the grid represent? 
  - 1’s are fresh oranges, 2’s are rotten oranges and 0’s are empty spaces
- What data structures can I use to store the grids? 
  - You can use a 2D array, hashset, queue, etc.
- Do we need to keep track of the level? 
  - Yes, you can keep track of the level using a search algorithm. Trick is to only increment once per level and only if fresh.
- What is a possible edge case? 
    - That there is no fresh, there is no rotten

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

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - BFS: We can model the grid in the form of a graph and to compute the distance for every node, we can apply standard BFS algorithm using a queue. We can think of it in a level by level manner. The key observation is that fresh oranges adjacent to rotten oranges are rotten on day 1, those adjacent to those oranges are rotten on day 2, and so on. The phenomenon is similar to a level order traversal on a graph, where all the initial rotten oranges act as root nodes. We can just push all these root nodes into a queue and perform BFS on a grid algorithm, to calculate the total time taken to rot all the oranges. Since there can be multiple rotten cells, we will push all those cells in the queue first and then continue with the BFS. If all oranges are not rotten before our algorithm terminates, we will return -1.
- Hash the 2D Array in some way to help with the Strings
    - We are not working with strings here
- Create/Utilize a Trie
    - A Trie will complicate the problem.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a queue data structure to keep track of the candidates that we need to visit during the process

```markdown
1. Initialize a queue for breadth first search.
2. Iterate over the entire grid and add all the rotten oranges in the queue and also keep counting the number of fresh oranges.
3. If the number of fresh oranges is zero then we can directly return zero.
4. Otherwise, traverse the queue in level order fashion and add all the adjacent fresh oranges in the queue and decrement the count of fresh oranges by 1 each time. When we add a fresh orange in the queue, we mark it as rotten so that it is not added multiple times.
5. If after one complete traversal of a level, the queue is not empty, then increase the minutes by one.
6. Repeat this process until we have no more rotten oranges.
7. If the number of fresh oranges after the entire process is still not zero, then return -1 indicating that it’s impossible to rot all the oranges.
8. Else return the time required to rot all the oranges.
```

⚠️ **Common Mistakes**

* Index-out-of-range error is common mistake that comes up if unable to design the matrix. Make sure to define a value to map uniquely the position in the matrix. Matrix has rows * columns elements, rows from 0 to rows - 1, columns from 0 to columns - 1.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
from collections import deque
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        queue = deque()

        # Step 1). build the initial set of rotten oranges
        fresh_oranges = 0
        ROWS, COLS = len(grid), len(grid[0])
        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == 2:
                    queue.append((r, c))
                elif grid[r][c] == 1:
                    fresh_oranges += 1

        # Mark the round / level, _i.e_ the ticker of timestamp
        queue.append((-1, -1))

        # Step 2). start the rotting process via BFS
        minutes_elapsed = -1
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        while queue:
            row, col = queue.popleft()
            if row == -1:
                # We finish one round of processing
                minutes_elapsed += 1
                if queue:  # to avoid the endless loop
                    queue.append((-1, -1))
            else:
                # this is a rotten orange
                # then it would contaminate its neighbors
                for d in directions:
                    neighbor_row, neighbor_col = row + d[0], col + d[1]
                    if ROWS > neighbor_row >= 0 and COLS > neighbor_col >= 0:
                        if grid[neighbor_row][neighbor_col] == 1:
                            # this orange would be contaminated
                            grid[neighbor_row][neighbor_col] = 2
                            fresh_oranges -= 1
                            # this orange would then contaminate other oranges
                            queue.append((neighbor_row, neighbor_col))

        # return elapsed minutes if no fresh orange left
        return minutes_elapsed if fresh_oranges == 0 else -1
```
```java
class Solution {
    public int orangesRotting(int[][] grid) {
        Queue<Pair<Integer, Integer>> queue = new ArrayDeque();

        // Step 1). build the initial set of rotten oranges
        int freshOranges = 0;
        int ROWS = grid.length, COLS = grid[0].length;

        for (int r = 0; r < ROWS; ++r)
            for (int c = 0; c < COLS; ++c)
                if (grid[r][c] == 2)
                    queue.offer(new Pair(r, c));
                else if (grid[r][c] == 1)
                    freshOranges++;

        // Mark the round / level, _i.e_ the ticker of timestamp
        queue.offer(new Pair(-1, -1));

        // Step 2). start the rotting process via BFS
        int minutesElapsed = -1;
        int[][] directions = { {-1, 0}, {0, 1}, {1, 0}, {0, -1}};

        while (!queue.isEmpty()) {
            Pair<Integer, Integer> p = queue.poll();
            int row = p.getKey();
            int col = p.getValue();
            if (row == -1) {
                // We finish one round of processing
                minutesElapsed++;
                // to avoid the endless loop
                if (!queue.isEmpty())
                    queue.offer(new Pair(-1, -1));
            } else {
                // this is a rotten orange
                // then it would contaminate its neighbors
                for (int[] d : directions) {
                    int neighborRow = row + d[0];
                    int neighborCol = col + d[1];
                    if (neighborRow >= 0 && neighborRow < ROWS && 
                        neighborCol >= 0 && neighborCol < COLS) {
                        if (grid[neighborRow][neighborCol] == 1) {
                            // this orange would be contaminated
                            grid[neighborRow][neighborCol] = 2;
                            freshOranges--;
                            // this orange would then contaminate other oranges
                            queue.offer(new Pair(neighborRow, neighborCol));
                        }
                    }
                }
            }
        }

        // return elapsed minutes if no fresh orange left
        return freshOranges == 0 ? minutesElapsed : -1;
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

* **Time Complexity**: O(N * M) For first traversal of the grid to find all the rotten and fresh oranges + O(n*m) for queue traversal if there is only one fresh orange and that too when the last orange is fresh.
* **Space Complexity**: O(N * M) Extra space for queue in the worst case when all the oranges are rotten.