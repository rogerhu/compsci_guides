## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/set-matrix-zeroes/](https://leetcode.com/problems/set-matrix-zeroes/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, String
* 🗒️ **Similar Questions**: [Game of Life](https://leetcode.com/problems/game-of-life/), [Number of Laser Beams in a Bank](https://leetcode.com/problems/number-of-laser-beams-in-a-bank/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What if the cell of the matrix has a zero?
  - If any cell of the matrix has a zero we can record its row and column number. All the cells of this recorded row and column can be marked zero in the next iteration. If the first cell of a row is set to zero this means the row should be marked zero. If the first cell of a column is set to zero this means the column should be marked zero.
- What if the cell[0][0] == 0?
  - If this is the case, we mark the first row as zero.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: matrix = [[1,0,1],[1,1,1],[1,0,1]]
Output: [[0,0,0],[1,0,1],[0,0,0]]

EDGE CASE
Input: [[1,0,1],[1,1,1],[1,1,1]]
Output: [[0,0,0],[1,0,1],[1,0,1]]
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can use the first row and first column of the matrix to store the status of the rows and columns respectively. The problem will occur for the status of first row and column which can be handled using two variables which will store the status of the first row and column.


```markdown
1) Initialize `firstRow` and `firstCol` to false. These two variables will store the status of the first row and first column.
2) Now use the first row and first column as your hash which stores the status of that row and column.
3) Iterate over the matrix and for every `A[i][j] == 0`, set `A[i][0] = 0` and `col[0][j] = 0`.
4) Update the values of the matrix except first row and first column to 0 if `A[i][0] = true` or `A[0][i] = true` for `A[i][j]`.
5) Update the values of the first row and first column.
```

⚠️ **Common Mistakes**

* Don't forget the additional flags: to check if all elements in the first row should be set into zero and another to check if all elements in the first column should be set into zero.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def setZeroes(self, matrix):
        is_col = False
        R = len(matrix)
        C = len(matrix[0])
        for i in range(R):
            # use an additional variable for the first column
            # and using matrix[0][0] for the first row.
            if matrix[i][0] == 0:
                is_col = True
            for j in range(1, C):
                # if an element is zero, we set the first element of the corresponding row and column to 0
                if matrix[i][j]  == 0:
                    matrix[0][j] = 0
                    matrix[i][0] = 0

        # iterate over the array once again and using the first row and first column, update the elements.
        for i in range(1, R):
            for j in range(1, C):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0

        # see if the first row needs to be set to zero as well
        if matrix[0][0] == 0:
            for j in range(C):
                matrix[0][j] = 0

        # see if the first column needs to be set to zero as well        
        if is_col:
            for i in range(R):
                matrix[i][0] = 0
```
```java
class Solution {
  public void setZeroes(int[][] matrix) {
    Boolean isCol = false;
    int R = matrix.length;
    int C = matrix[0].length;

    for (int i = 0; i < R; i++) {

      // use an additional variable for the first column
      // and using matrix[0][0] for the first row.
      if (matrix[i][0] == 0) {
        isCol = true;
      }

      for (int j = 1; j < C; j++) {
        // if an element is zero, we set the first element of the corresponding row and column to 0
        if (matrix[i][j] == 0) {
          matrix[0][j] = 0;
          matrix[i][0] = 0;
        }
      }
    }

    // iterate over the array once again and using the first row and first column, update the elements.
    for (int i = 1; i < R; i++) {
      for (int j = 1; j < C; j++) {
        if (matrix[i][0] == 0 || matrix[0][j] == 0) {
          matrix[i][j] = 0;
        }
      }
    }

    // see if the first row needs to be set to zero as well
    if (matrix[0][0] == 0) {
      for (int j = 0; j < C; j++) {
        matrix[0][j] = 0;
      }
    }

    // see if the first column needs to be set to zero as well
    if (isCol) {
      for (int i = 0; i < R; i++) {
        matrix[i][0] = 0;
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
    
* **Time Complexity**: O(M×N), where M and N are the number of rows and columns respectively

* **Space Complexity**: O(1)
