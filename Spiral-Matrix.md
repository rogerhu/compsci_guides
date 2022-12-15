## Problem Highlights

* 🔗 **Leetcode Link:** [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, 2D-Array, DFS, BFS
* 🗒️ **Similar Questions**: [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/), [Spiral Matrix III](https://leetcode.com/problems/spiral-matrix-iii/), [Spiral Matrix IV](https://leetcode.com/problems/spiral-matrix-iv/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input be empty List?

  - The size range of the List is from 1 to 10 in both size. Therefore the input can never be empty.


```markdown
HAPPY CASE
Input: [
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]

Input: [
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9, 10, 11, 12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]

EDGE CASE
Input: [
  [1],
  [2],
  [3],
  [4]
]
Output: [1,2,3,4]

```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - DFS: The problem statement asks us to return all elements of the matrix in spiral order, which means we will start from the top left corner and move towards right, then down, then left, and then up. This problem is a stimulation problem, so we need to follow the spiral order, and keep track of the boundaries of the matrix. DFS will do this for us.

- Hash the 2D Array in some way to help with the Strings
    - We are not working with strings
- Create/Utilize a Trie
    - A Trie will complicate the problem



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** When we finish traversing a row or column, we want to set up a boundary on it so that the next time we get there, we know we need to change the direction.


```markdown
1. Initialize the output array result.
2. Initialize the top, right, bottom, and left boundaries as up, right, down, and left.
3. Traverse the elements in spiral order and add each element to result:
4. Traverse from left boundary to right boundary.
5. Traverse from up boundary to down boundary.
6. Before we traverse from right to left, we need to make sure that we are not on a row that has already been traversed. If we are not, then we can traverse from right to left.
7. Similarly, before we traverse from top to bottom, we need to make sure that we are not on a column that has already been traversed. Then we can traverse from down to up.
8. updating left, right, up, and down accordingly.
9. Return result.
```

⚠️ **Common Mistakes**

* How do you avoid having the recursion relation going on forever? We can start sweeps from top right and bottom left. With min(up, left) first, then this part of recursion relation ends at bottom right. After that, min(right, down) in the second sweep to end the recursion relation at top left.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        # 1. Initialize the output array result
        result = []

        # 2. Initialize the top, right, bottom, and left boundaries as up, right, down, and left
        rows, columns = len(matrix), len(matrix[0])
        up = left = 0
        right = columns - 1
        down = rows - 1

        # 3. Traverse the elements in spiral order and add each element to result:
        while len(result) < rows * columns:
            # 4. Traverse from left boundary to right boundary.
            for col in range(left, right + 1):
                result.append(matrix[up][col])

            # 5. Traverse from up boundary to down boundary.
            for row in range(up + 1, down + 1):
                result.append(matrix[row][right])

            # 6. Make sure that we are not on a row that has already been traversed
            if up != down:
                # Traverse from right to left.
                for col in range(right - 1, left - 1, -1):
                    result.append(matrix[down][col])

            # 7. Make sure that we are not on a column that has already been traversed
            if left != right:
                # Traverse from down to up.
                for row in range(down - 1, up, -1):
                    result.append(matrix[row][left])

            # 8. updating left, right, up, and down accordingly.
            left += 1
            right -= 1
            up += 1
            down -= 1

        # 9. Return result.
        return result
```
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        // 1. Initialize the output array result.
        List<Integer> result = new ArrayList<>();

        // 2. Initialize the top, right, bottom, and left boundaries as up, right, down, and left.
        int up = 0;
        int left = 0;
        int right = matrix[0].length - 1;
        int down = matrix.length - 1;

        // 3. Traverse the elements in spiral order and add each element to result:
        while (result.size() < matrix.length * matrix[0].length) {
            // 4. Traverse from left boundary to right boundary.
            for (int col = left; col <= right; col++) {
                result.add(matrix[up][col]);
            }
            // 5. Traverse from up boundary to down boundary.
            for (int row = up + 1; row <= down; row++) {
                result.add(matrix[row][right]);
            }
            // 6. Make sure that we are not on a row that has already been traversed
            if (up != down) {
                // Traverse from right to left.
                for (int col = right - 1; col >= left; col--) {
                    result.add(matrix[down][col]);
                }
            }
            // 7. Make sure that we are not on a column that has already been traversed
            if (left != right) {
                // Traverse from down to up.
                for (int row = down - 1; row > up; row--) {
                    result.add(matrix[row][left]);
                }
            }
            // 8. updating left, right, up, and down accordingly.
            left++;
            right--;
            up++;
            down--;
        }
        // 9. Return result.
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
Assume`M` represents the number of columns in 2D-array.


* **Time Complexity**: O(N * M) we need to view each item in the 2D-Array
* **Space Complexity**: O(1) we only have a few pointers for iterating. 
    * Do note the recusive DFS solution will cost us O(N*M) space because of the call stack. 