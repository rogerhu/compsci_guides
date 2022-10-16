🔗 **Leetcode Link:** [https://leetcode.com/problems/word-search](https://leetcode.com/problems/word-search)

⏰ **Time to complete**: 11 mins

1. **U-nderstand**

a. When do we return true? Return true when the last letter is reached.

b. What if we come across the same letter during the search path? During the search path, set the visited letter as visited to avoid reuse.

```markdown
word = "ABCCED", -> returns 1,

word = "SEE", -> returns 1,

word = "ABCB", -> returns 1,

word = "ABFSAB" -> returns 1

word = "ABCD" -> returns 0
```

1. M-atch
    
    For graph problems, some things we want to consider are:
    
    Our goal is to find if the `word` exists in the matrix or not. We only have to look at the adjacent cells (ignore the diagonal ones). Match character-by-character, go ahead and check if the adjacent cells match the next character, and go back if it does not match.
    
    How should we traverse the matrix efficiently? We need to think of a traversal approach. BFS? DFS? Both can work. But DFS will be better as it immediately checks the next node of the graph and returns if it is not needed after marking it as visited.
    
2. P-lan
    1. Create helper function(node, string):
        1. If the string is empty, return true (success).
        2. If the node's letter is not the first letter of the string, return false (failure).
        3. Otherwise, return true if the helper function returns true for
            1. any neighbor
            2. the remainder of the string
    2. Implement main function(board, string):
        1. Return true if any of these helper function calls return true
            1. each node
            2. the full string
3. I-mplement

```python
def exist(self, board: List[List[str]], word: str) -> bool:        
        if not board or not word:
            return False    
    
        def dfs(row, col, index):
            if index == len(word):
                return True
            
            if row < 0 or row >= len(board) or col < 0 or col >= len(board[row]) or board[row][col] != word[index]:
                return False
            
            temp = board[row][col]
            board[row][col] = ''
            found = dfs(row + 1, col, index + 1) or dfs(row - 1, col, index + 1) or dfs(row, col + 1, index + 1) or dfs(row, col - 1, index + 1)
            board[row][col] = temp
            return found
        
        for row in range(len(board)):
            for col in range(len(board[row])):
                if word[0] == board[row][col] and dfs(row, col, 0):
                    return True

        return False
```

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] arr = word.toCharArray();
        boolean[][] visited = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                visited[i][j] = true;
                if (dfs(board,visited, arr,0, i, j)) {
                    return true;
                } else {
                    visited[i][j] = false;
                }
            }
        }
        return false;
        
    }
    
    private boolean dfs(char[][] board, boolean[][] visited, char[] arr, int index, int row, int col) {
        if (arr[index] != board[row][col]) {
            return false;
        } 
        if (index == arr.length - 1) {
            return true;
        }
        int[] rHelp = new int[]{1,-1,0,0};
        int[] cHelp = new int[]{0,0,1, -1};
        for (int i = 0; i < 4; i++) {
            int nextRow = row + rHelp[i];
            int nextCol = col + cHelp[i];
            if (isValid(board, visited, nextRow, nextCol)) {
                visited[nextRow][nextCol] = true;
                if (dfs(board,visited, arr, index + 1, nextRow, nextCol)) {
                    return true;
                } else {
                    visited[nextRow][nextCol] = false;
                }
            }
        }
        return false;
    }
    
    private boolean isValid(char[][] board, boolean[][] visited, int row, int col) {
        int rowMax = board.length - 1;
        int colMax = board[0].length - 1;
        if (row >= 0 && row <= rowMax && col >= 0 && col <= colMax && !visited[row][col]) {
            return true;
        } else {
            return false;
        }
    }
}
```

1. R-eview

Verify the code works for the happy and edge cases you created in the “Understand” section

1. E-valuate
    
    **Time Complexity: O(m * n * 4^w)**
    
    Board size = n * m. w - length of the word
    
    **Space Complexity: O(1)**
    
    The space complexity would be the length of the word, since the recursion stack only goes as far as the length. However, if you are using extra space to mark visited cells, then it would be the size of the board.