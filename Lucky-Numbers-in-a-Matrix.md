## Problem Highlights

* 🔗 **Leetcode Link:** [Lucky Numbers in a Matrix](https://leetcode.com/problems/lucky-numbers-in-a-matrix/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, 2D-Array
* 🗒️ **Similar Questions**: [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/), [Flipping an Image](https://leetcode.com/problems/flipping-an-image/), [Rotate Image](https://leetcode.com/problems/Rotate-Image/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input grid be blank??
    - Let’s assume the grid is not blank. We don’t need to consider empty inputs.

- What are the time and space constraints?
    - Time complexity should be `O(m*n)`, m being the rows of the matrix and n being the columns of matrix. Space complexity should be `O(m*n)` too.

```markdown
HAPPY CASE
Input: matrix = [[3,7,8],[9,11,13],[15,16,17]]
Output: [15]
Explanation: 15 is the only lucky number since it is the minimum in its row and the maximum in its column.

nput: matrix = [[1,10,4,2],[9,3,8,7],[15,16,17,12]]
Output: [12]
Explanation: 12 is the only lucky number since it is the minimum in its row and the maximum in its column.

EDGE CASE

Input: matrix = [[1]]
Output: [1]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - A search through the 2D Array (either BFS or DFS) does not help us. We need to find min values in rows and max values in columns. We do not need to go in all four directions.

- Hash the 2D Array in some way to help with the Strings
    - Hashing would not help us here.

- Create/Utilize a Trie
    - A Trie would not help us much in this problem since we are not trying to determine anything about a sequence of characters.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create a set of minimum number in each row and a set of maximum number in each column. If a number is in both set, then it is both the minimum number in the row and maximum number in the column.

```markdown
1) Create variables to hold minimum row numbers and maximum column numbers.
2) Search each row in matrix for minimum number and create minimum row numbers set
3) Search each column in matrix for maximum number and create maximum column numbers set
    a) Transpose the matrix using zip(*matrix) to create columns
        - For more information see https://github.com/codepath/compsci_guides/wiki/Transpose-Matrix
    b) Store maximum number per column in set
4) Return a list of numbers that is in both sets. 
```

⚠️ **Common Mistakes**
* Not every 2D-Array problem follows the common techniques.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Solution 1:**

```python
class Solution:
    def luckyNumbers (self, matrix: List[List[int]]) -> List[int]:
        # Create variables to hold minimum row numbers and maximum column numbers
        minRow, maxCol = set(), set()

        # Search each row in matrix for minimum number and create minimum row numbers set
        for row in matrix:
            minRow.add(min(row))
        
        # Search each column in matrix for maximum number and create maximum column numbers set
        # Transpose the matrix using zip(*matrix) to create columns
        for col in zip(*matrix):
            # Store maximum number per column in set
            maxCol.add(max(col))
        
        # Return a list of numbers that is in both sets
        return list(minRow & maxCol)
```


## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of rows in 2D-array.
Assume`M` represents the number of columns in 2D-array.


* **Time Complexity**: O(N * M) we need to view each item in the 2D-Array to get the minimum number in row and maximum number in column. 
* **Space Complexity**: O(N * M), because we need space to hold the results. Results may contain all items in 2D-Array. 