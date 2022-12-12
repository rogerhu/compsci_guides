## Problem Highlights

* 🔗 **Leetcode Link:** [Word Search](https://leetcode.com/problems/word-search)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Array, 2D-Array, DFS, BFS, Backtracking
* 🗒️ **Similar Questions**: [Word Search II](https://leetcode.com/problems/word-search-ii/), [Unique Paths III](https://leetcode.com/problems/unique-paths-iii/), [Encrypt and Decrypt Strings](https://leetcode.com/problems/encrypt-and-decrypt-strings/)
    
## 1: U-nderstand
 
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

- What is the smallest 2D-Array?
    - 1 row and 1 column
    
- What is the shortest word?
    - 1 character long
    
- Is it possible that the word is longer than then number of possible squares in 2D-Array?
    - Yes

```markdown
HAPPY CASE
Input:  board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true

Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
Output: true

EDGE CASE
Input: board = [["A","B","C"],["S","F","C"],["A","D","E"]], word = "ABCSFCADEA"
Output: false
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - DFS: Our goal is to find if the word exists in the matrix or not. We only have to look at the adjacent cells (ignore the diagonal ones). Match character-by-character, go ahead and check if the adjacent cells match the next character, and go back if it does not match. How should we traverse the matrix efficiently? We need to think of a traversal approach. BFS? DFS? Both can work. But DFS will be better as it immediately checks the next position and returns if it is not needed after marking it as visited.
 

- Hash the 2D Array in some way to help with the Strings
    - We do need a visited hashset to ensure that we do not revisit the same position twice.
- Create/Utilize a Trie
    - A Trie will complicate the problem.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Apply DFS/Backtracking at each position in the 2D-Array to see if we can create the desired word, with each DFS/Backtracking call we can detect a possible match or return to previous DFS and try next branch. 



```markdown
0. Preliminary Checks
    a. Check if word has more characters than board

1. Define a helper function to check if a word exists starting at a given position
    a. Check if the index is out of bounds or the character does not match, return false (failure).
    b. If we have reached the end of the word, return True
    c. Mark the current position as valid and visited by replacing it with a space character
    d. Check if the rest of the word exists in any of the adjacent positions
    e. Backtrack: Restore the original character at the current position
    e. Return the result of the search
    
2. Implement main function(board, string):
    a. Check each starting point for the full string using DFS, return true if found
    b. Otherwise have checked each position and none of them will spell our word  
```

⚠️ **Common Mistakes**

* How do you avoid having the Time Limit Exceeded?
    * Perform the prelimiary checks and make sure the word can fit inside the 2D-Array. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        # Get the dimensions of the board
        rows = len(board)
        cols = len(board[0])
        
        # Check if word has more characters than board
        if rows*cols < len(word):
            return False

        # Define a helper function to check if a word exists starting at a given position
        def check(i, j, k):
            # Check if the index is out of bounds or the character does not match
            if i < 0 or i >= rows or j < 0 or j >= cols or board[i][j] != word[k]:
                return False
            
            # If we have reached the end of the word, return True
            if k == len(word) - 1:
                return True
            
            # Mark the current position as visited by replacing it with a space character
            temp = board[i][j]
            board[i][j] = ' '
            
            # Check if the rest of the word exists in any of the adjacent positions
            found = check(i-1, j, k+1) or check(i+1, j, k+1) or check(i, j-1, k+1) or check(i, j+1, k+1)
            
            # Backtrack: Restore the original character at the current position
            board[i][j] = temp
            
            # Return the result of the search
            return found
        
        # Check each starting point for the full string using DFS, return true if found
        for i in range(len(board)):
            for j in range(len(board[0])):
                if check(i, j, 0):
                    return True
        # Otherwise have checked each position and none of them will spell our word
        return False
```

```java
class Solution {
    // Implement main function(board, string)   
    public boolean exist(char[][] board, String word) {
        // 0. Preliminary Checks
        // Check if word has more characters than board
        if (board.length * board[0].length < word.length()) {
            return false;
        }

        // Check each starting point for the full string using DFS, return true if found
        char[] arr = word.toCharArray();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(board, arr,0, i, j)) {
                    return true;
                } 
            }
        }
        // Otherwise have checked each position and none of them will spell our word  
        return false;
    }
  
  private boolean dfs(char[][] board, char[] arr, int index, int row, int col) {
    //  Check if the index is out of bounds or the character does not match, return false (failure).
    int rowMax = board.length - 1;
    int colMax = board[0].length - 1;
    if (row < 0 || row > rowMax || col < 0 || col > colMax || arr[index] != board[row][col]) {
      return false;
    } 
    // If we have reached the end of the word, return True
    if (index == arr.length - 1) {
      return true;
    }
    // Mark the current position as valid and visited by replacing it with a space character
    char temp = board[row][col];
    board[row][col] = '#';

    // Check if the rest of the word exists in any of the adjacent positions
    int[] rHelp = new int[]{1,-1,0,0};
    int[] cHelp = new int[]{0,0,1, -1};
    for (int i = 0; i < 4; i++) {
        int nextRow = row + rHelp[i];
        int nextCol = col + cHelp[i];
        if (dfs(board, arr, index + 1, nextRow, nextCol)) {
            return true;
        } 
    }
    // Backtrack: Restore the original character at the current position
    board[row][col] = temp;

    // Return the result of the search
    return false;
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
Assume `L` represents the number of characters in the word.

* **Time Complexity**: O(N * M * 3 ^ L) we need to start at each character in the 2D-Array. Then we need to perform a dfs on 3 possible paths for the word. And we need to do this at each character.
* **Space Complexity**: O(L) we may need to store the length of our word as depth in our recursion stack.  