## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/word-search](https://leetcode.com/problems/word-search)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: [Word Search II](https://leetcode.com/problems/word-search-ii/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- When do we return true? 
  - Return true when the last letter is reached.

- What if we come across the same letter during the search path? 
  - During the search path, set the visited letter as visited to avoid reuse.

```markdown
word = "ABCCED", -> returns 1,

word = "SEE", -> returns 1,

word = "ABCB", -> returns 1,

word = "ABFSAB" -> returns 1

word = "ABCD" -> returns 0
```

## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- BFS/DFS: Our goal is to find if the `word` exists in the matrix or not. We only have to look at the adjacent cells (ignore the diagonal ones). Match character-by-character, go ahead and check if the adjacent cells match the next character, and go back if it does not match.
How should we traverse the matrix efficiently? We need to think of a traversal approach. BFS? DFS? Both can work. But DFS will be better as it immediately checks the next node of the graph and returns if it is not needed after marking it as visited.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Iterate over every element of grid checking if it is equals to first element of given word. If found then we call the DFS function with current position of board with starting position of word.

1. Create helper function(node, string):
    - if the string is empty, return true (success).
    - If the node's letter is not the first letter of the string, return false (failure).
    - Otherwise, return true if the helper function returns true for
       - any neighbor
       - the remainder of the string
2. Implement main function(board, string):
    - Return true if any of these helper function calls return true

⚠️ **Common Mistakes**

* Ensure you are marking the board cell visited. If we don't do so, then we might be end up comparing two or more characters of the string with the single character of the board. TLE can be avoided by passing board by reference so as to avoid making copy of board again and again when the function is invoked.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        
        if not board: 
            return False
        
        h, w = len(board), len(board[0])
      
        def dfs_search(idx: int, x: int, y: int) -> bool:
            
            if x < 0 or x == w or y < 0 or y == h or word[idx] != board[y][x]:
                # Reject if out of boundary, or current grid cannot match the character word[idx]
                return False

            if idx == len(word) - 1: 
                # Accept when we match all characters of word during DFS
                return True

            cur = board[y][x]
            
            # mark as '#' to avoid repeated traversal
            board[y][x] = '#'
            
            # visit next four neighbor grids
            found = dfs_search(idx + 1, x + 1, y) or dfs_search(idx + 1, x - 1, y) or dfs_search(idx + 1, x, y + 1) or dfs_search(idx + 1, x, y - 1)
            
            # recover original grid character after DFS is completed
            board[y][x] = cur
            
            return found
        
        return any(dfs_search(0, x, y) for y in range(h) for x in range(w))    
```

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] arr = word.toCharArray();
        for(int i = 0; i<board.length; i++) {
            for(int j = 0; j<board[i].length; j++) {
                if(dfs(i, j, board, arr, 0)) {
                    return true;   
                }
            }
        }
        return false;
    }
    
    private boolean dfs(int i, int j, char[][] board, char[] arr, int index) {
        if( index == arr.length) {
            return true;
        }
        Boolean result;
        if( 0 <= i && i < board.length && 
                0 <= j && j < board[i].length &&
                board[i][j] == arr[index]) {
				
            // mark this position as visited for the current DFS
			board[i][j] = '.';       
            result = dfs(i+1, j, board, arr, index+1) || 
                     dfs(i-1, j, board, arr, index+1) || 
                     dfs(i, j+1, board, arr, index+1) || 
                     dfs(i, j-1, board, arr, index+1);   
             // remove mark on the position and restore original character
			 board[i][j] = arr[index];
            return result;
        }
        return false;        
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section


## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
Time Complexity: `O(m * n * 4^w)`, where Board size = n * m and w = length of the word
<br>
Space Complexity: `O(1)`. The space complexity would be the length of the word, since the recursion stack only goes as far as the length. However, if you are using extra space to mark visited cells, then it would be the size of the board.