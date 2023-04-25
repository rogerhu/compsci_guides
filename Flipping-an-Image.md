## Problem Highlights

* 🔗 **Leetcode Link:** [Flipping an Image](https://leetcode.com/problems/flipping-an-image/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, 2D-Array
* 🗒️ **Similar Questions**: [Rotate Image](https://leetcode.com/problems/Rotate-Image/), [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input grid be blank??
    - Let’s assume the grid is not blank. We don’t need to consider empty inputs.
- Can the row size be different from the column size?
    - Yes, the row size can be different from the column size.
- What are the time and space constraints?
    - Time complexity should be `O(m*n)`, m being the rows of the array and n being the columns of array. Space complexity should be `O(1)`.

```markdown
HAPPY CASE
Input: image = [[1,1,0],[1,0,1],[0,0,0]]
Output: [[1,0,0],[0,1,0],[1,1,1]]
Explanation: First reverse each row: [[0,1,1],[1,0,1],[0,0,0]].
Then, invert the image: [[1,0,0],[0,1,0],[1,1,1]]

```markdown
Input: image = [[1,1,0,0],[1,0,0,1],[0,1,1,1],[1,0,1,0]]
Output: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]
Explanation: First reverse each row: [[0,0,1,1],[1,0,0,1],[1,1,1,0],[0,1,0,1]].
Then invert the image: [[1,1,0,0],[0,1,1,0],[0,0,0,1],[1,0,1,0]]

EDGE CASE

Input: matrix = [[1]]
Output: [[1]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - A search through the 2D Array (either BFS or DFS) does not help us. We are flipping a image horizontally, then inverting it, not searching.
- Hash the 2D Array in some way to help with the Strings
    - Hashing would not help us flipping a image horizontally, then inverting it
- Create/Utilize a Trie
    - A Trie would not help us much in this problem since we are not trying to determine anything about a sequence of characters.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Let's reverse each row, then flip every 1 to 0 and 0 to 1 in-place.

```markdown
1) Use the reverse function on each row
2) For each row flip each item from 0 to 1 and 0 to 1
```

⚠️ **Common Mistakes**
* Not every 2D-Array problem follows the common techniques.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def flipAndInvertImage(self, image: List[List[int]]) -> List[List[int]]:
        # Use the reverse function on each row
        for row in image:
            row.reverse()

            # For each row flip each item from 0 to 1 and 0 to 1.
            for i, element in enumerate(row):
                if element == 1:
                    row[i] = 0
                else:
                    row[i] = 1
        
        return image
```
```java
class Solution {
    public int[][] flipAndInvertImage(int[][] A) {
        int row = A.length;
        int col = A[0].length;
        int[][] result = new int[row][col];
        
		// Use the reverse function on each row
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                result[i][j] = A[i][col-j-1];
            }
        }
        // For each row flip each item from 0 to 1 and 0 to 1.
        for(int i = 0; i < row; i++){
            for(int j = 0; j < col; j++){
                result[i][j] = result[i][j] == 1 ? 0 : 1;
            }
        }
        return result;
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
Assume `M` represents the number of columns in 2D-array.


* **Time Complexity**: O(N * M) we need to flip each item in the 2D-Array
* **Space Complexity**: O(1), because we are not using any additional space, we flip and invert the image in-place.