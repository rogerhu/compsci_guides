## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/n-queens/>
* 💡 **Difficulty:** Hard
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Backtracking
* 🗒️ **Similar Questions**: [N-Queens II](https://leetcode.com/problems/n-queens-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Where can a queen move?
  - A queen can attack horizontally, vertically, or diagonally. A queen can move any number of steps in any direction. The only constraint is that it can’t change its direction while it’s moving. 
- Can a queen be in the same row or column?
  No two queens can be in the same row or column. That allows us to place only one queen in each row and each column.  
- 

Run through a set of example cases:

```markdown
Example 1:
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]

Example 2:
Input: n = 1
Output: [["Q"]]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

* How can N queens be placed on an NxN chessboard so that no two of them attack each other? Backtracking can be used since we start with one pos­si­ble move out of many avail­able moves. The three general steps include: the general steps of backtracking are: start with a sub-solution
check if this sub-solution will lead to the solution or not. If not, then come back and change the sub-solution and continue again.

The way we try to solve this is by placing a queen at a position and trying to rule out the possibility of it being under attack. We place one queen in each row/column. If we see that the queen is under attack at its chosen position, we try the next position. If a queen is under attack at all the positions in a row, we backtrack and change the position of the queen placed prior to the current position. We repeat this process of placing a queen and backtracking until all the N queens are placed successfully.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1) Initialize an empty chessboard of size NxN.
2) Start with the leftmost column and place a queen in the first row of that column.
3) Move to the next column and place a queen in the first row of that column.
4) Repeat step 3 until either all N queens have been placed or it is impossible to place a queen in the current column without violating the rules of the problem. If all N queens have been placed, print the solution. If it is not possible to place a queen in the current column without violating the rules of the problem, backtrack to the previous column.
5) Remove the queen from the previous column and move it down one row.
6) Repeat steps 4-7 until all possible configurations have been tried.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

* For an 8×8 board, we have to choose positions for 8 identical queens from 64  different squares, which can be done in 64C8 = 4426165368 ways. You can tighten this up by enforcing one queen in each row through the use of 8 nested loop. Using such nested loops, we would only have to look at 44=256 configurations for a 4×4 board and 88 configurations for an 8×8 board --- that's still far too many. 
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def solveNQueens(self, n):
        # Making use of a helper function to get the
        # solutions in the correct output format
        def create_board(state):
            board = []
            for row in state:
                board.append("".join(row))
            return board
        
        def backtrack(row, diagonals, anti_diagonals, cols, state):
            # Base case - N queens have been placed
            if row == n:
                ans.append(create_board(state))
                return

            for col in range(n):
                curr_diagonal = row - col
                curr_anti_diagonal = row + col
                # If the queen is not placeable
                if (col in cols 
                      or curr_diagonal in diagonals 
                      or curr_anti_diagonal in anti_diagonals):
                    continue

                # "Add" the queen to the board
                cols.add(col)
                diagonals.add(curr_diagonal)
                anti_diagonals.add(curr_anti_diagonal)
                state[row][col] = "Q"

                # Move on to the next row with the updated board state
                backtrack(row + 1, diagonals, anti_diagonals, cols, state)

                # "Remove" the queen from the board since we have already
                # explored all valid paths using the above function call
                cols.remove(col)
                diagonals.remove(curr_diagonal)
                anti_diagonals.remove(curr_anti_diagonal)
                state[row][col] = "."

        ans = []
        empty_board = [["."] * n for _ in range(n)]
        backtrack(0, set(), set(), set(), empty_board)
        return ans
```
```java
class Solution {
    private int size;
    private List<List<String>> solutions = new ArrayList<List<String>>();
    
    public List<List<String>> solveNQueens(int n) {
        size = n;
        char emptyBoard[][] = new char[size][size];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                emptyBoard[i][j] = '.';
            }
        }

        backtrack(0, new HashSet<>(), new HashSet<>(), new HashSet<>(), emptyBoard);
        return solutions;
    }
    
    // Making use of a helper function to get the
    // solutions in the correct output format
    private List<String> createBoard(char[][] state) {
        List<String> board = new ArrayList<String>();
        for (int row = 0; row < size; row++) {
            String current_row = new String(state[row]);
            board.add(current_row);
        }
        
        return board;
    }
    
    private void backtrack(int row, Set<Integer> diagonals, Set<Integer> antiDiagonals, Set<Integer> cols, char[][] state) {
        // Base case - N queens have been placed
        if (row == size) {
            solutions.add(createBoard(state));
            return;
        }
        
        for (int col = 0; col < size; col++) {
            int currDiagonal = row - col;
            int currAntiDiagonal = row + col;
            // If the queen is not placeable
            if (cols.contains(col) || diagonals.contains(currDiagonal) || antiDiagonals.contains(currAntiDiagonal)) {
                continue;    
            }
            
            // "Add" the queen to the board
            cols.add(col);
            diagonals.add(currDiagonal);
            antiDiagonals.add(currAntiDiagonal);
            state[row][col] = 'Q';

            // Move on to the next row with the updated board state
            backtrack(row + 1, diagonals, antiDiagonals, cols, state);

            // "Remove" the queen from the board since we have already
            // explored all valid paths using the above function call
            cols.remove(col);
            diagonals.remove(currDiagonal);
            antiDiagonals.remove(currAntiDiagonal);
            state[row][col] = '.';
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


Given n as the number of queens (which is the same as the width and height of the board),

* **Time Complexity**: O(n!), because we are selecting if we can put or not put a Queen at that place
* **Space Complexity**: O(n^2), extra memory used includes the 3 sets used to store board state, as well as the recursion call stack
