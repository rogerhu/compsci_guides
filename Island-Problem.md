## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Islands](https://leetcode.com/problems/number-of-islands/)
* 💡 **Problem Difficulty:** Medium
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


```markdown
HAPPY CASE
Input:
11110
11010
11000
00000

Output:  1

HAPPY CASE

Input:
11000
11000
00100
00011

Output: 3

EDGE CASE

Input:
00000
00000
00000
00000

Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - A search through the 2D Array (either BFS or DFS) can help us find and count the islands. Which of these two traversals will better help us locate islands?

- Hash the 2D Array in some way to help with the Strings
    - Hashing would not directly help us find islands. However, we could hash where certain 1's are in the 2D Array to jumpstart searches (BFS/DFS) faster.
- Create/Utilize a Trie
    - A Trie would not help us much in this problem since we are not trying to determine anything about a sequence of characters.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Perform a DFS for islands in the grid.

```markdown
main function:
1) Initialize a variable to keep track of the number of islands
2) Iterate over the grid
3) If a '1' is seen:
    a) increment a variable counter
    b) "explore" the island from this '1' using dfs
4) Return variable counter

dfs function:
1) Change the grid value to '0' to mark it as visited
2) Recursively check 4 neighbors from the current cell for more island cells (1's).
  This allows us to explore that island completely.
```

⚠️ **Common Mistakes**
* It can be easy to miss the underlying traversal problem. Make sure to think about how you can optimize traversing the input grid and use the structure to your advantage.
* Some people might forget to use a visited set or some other mechanism to keep track of what coordinates with 1's have already been visited.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        dr = [0, -1, 0, 1]
        dc = [-1, 0, 1, 0]

        def sink(r, c):
            # Change the grid value to '0' to mark it as visited
            grid[r][c] = '0'
            # Recursively check 4 neighbors from the current cell for more island cells (1's).
            for d in range(4):
                new_r, new_c = r + dr[d], c + dc[d]
                if 0 <= new_r < len(grid) and 0 <= new_c < len(grid[0]) and grid[new_r][new_c] == '1':
                    sink(new_r, new_c)

        # Initialize a variable to keep track of the number of islands
        count = 0

        # Iterate over the grid
        for r in range(len(grid)):
            for c in range(len(grid[r])):
                # If a '1' is seen:
                if grid[r][c] == '1':
                    # increment a variable counter and "explore" the island from this '1' using dfs
                    count += 1
                    sink(r, c)
        # Return variable counter
        return count
```
```java
public class Solution {
    private int n;
    private int m;
    private int[] dr = {0, -1, 0, 1};
    private int[] dc = {-1, 0, 1, 0};

    public int numIslands(char[][] grid) {
        // Initialize a variable to keep track of the number of islands
        int count = 0;

        // Iterate over the grid
        n = grid.length;
        if (n == 0) return 0;
        m = grid[0].length;
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < m; c++) {
                // If a '1' is seen:
                if (grid[r][c] == '1') {
                    // increment a variable counter and "explore" the island from this '1' using dfs
                    count++;
                    sink(grid, r, c);
                }
            }
        }    
        // Return variable counter
        return count;
    }

    private void sink(char[][] grid, int r, int c) {
        // Change the grid value to '0' to mark it as visited
        grid[r][c] = '0';
        // Recursively check 4 neighbors from the current cell for more island cells (1's).
        for (int d = 0; d < 4; d++) {
            int new_r = r + dr[d], new_c = c + dc[d];
            if (new_r >= 0 && new_r < n && new_c >= 0 && new_c < m && grid[new_r][new_c] == '1') {
                sink(grid, new_r, new_c);
            }
        } 
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
* **Space Complexity**: O(N * M) the recusive DFS solution will cost us O(N*M) space because we may place the entire grid in the the recursive call stack. 