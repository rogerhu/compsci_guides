## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/maximal-rectangle/>
* 💡 **Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/), [Maximal Square](https://leetcode.com/problems/maximal-square/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input be empty?
  - Yes, the input can be empty. In that case, let’s return 0 since there is no rectangle.
- What is the maximal width of a rectangle?
  - The maximal width of a rectangle spanning from the original point to the current point is the running minimum of each maximal width.
- 
   
```markdown
HAPPY CASE
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6

EDGE CASE (Single Element)
Input: 
[
  ["1"]
]
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

* Whenever you see a question that asks for the maximum or minimum of something, consider Dynamic Programming as a possibility. 

* Since this problem tends to look like a Dynamic Programming problem, let’s try to match this problem with standard DP techniques we may have seen before:

`2D Array Top-Left -> Bottom-Right, Bottom-Up DP Technique`:
The square verison of this problem was solved through this pattern. However, now that we can create rectangles with unequal side lengths, this may create an issue with this solution pattern.

* Knapsack-Type Approach
There isn’t a Knapsack underlying problem-type to this problem.
There may be other DP techniques not covered here that may seem fit to discuss

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

We can compute the maximum width of a rectangle that ends at a given coordinate in constant time. We do this by keeping track of the number of consecutive ones each square in each row.

```markdown
Compute Largest Rectangle Algorithm
1) Create a stack to store ascending heights of row
2) Iterate through the row from left to right
    a) While the current index height is smaller than the top of the stack height
        i) Compute the area of the current index to the top of the stack index and 
            update the max value variable
            - NOTE: Computation of area requires more specific details
    b) Push current index onto stack
3) Return largest rectangle area for this row
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

* Solving this problem by considering each and every possible rectangle and considering the maximal out of the rectangles which only consists of 1 in them is a brute force way.  Instead of forming every rectangle, then checking validity of rectangle, we can optimize the brute-force by only considering valid rectangles.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        maxarea = 0

        dp = [[0] * len(matrix[0]) for _ in range(len(matrix))]
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == '0': continue

                # compute the maximum width and update dp with it
                width = dp[i][j] = dp[i][j-1] + 1 if j else 1

                # compute the maximum area rectangle with a lower right corner at [i, j]
                for k in range(i, -1, -1):
                    width = min(width, dp[k][j])
                    maxarea = max(maxarea, width * (i-k+1))
        return maxarea

```
```java
class Solution {
    public int maximalRectangle(char[][] matrix) {

        if (matrix.length == 0) return 0;
        int maxarea = 0;
        int[][] dp = new int[matrix.length][matrix[0].length];

        for(int i = 0; i < matrix.length; i++){
            for(int j = 0; j < matrix[0].length; j++){
                if (matrix[i][j] == '1'){

                    // compute the maximum width and update dp with it
                    dp[i][j] = j == 0? 1 : dp[i][j-1] + 1;

                    int width = dp[i][j];

                    // compute the maximum area rectangle with a lower right corner at [i, j]
                    for(int k = i; k >= 0; k--){
                        width = Math.min(width, dp[k][j]);
                        maxarea = Math.max(maxarea, width * (i - k + 1));
                    }
                }
            }
        } return maxarea;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N^2M) Computing the maximum area for one point takes O(N) time, since it iterates over the values in the same column. 
* **Space Complexity**: O(NM). We allocate an equal sized array to store the maximum width at each point.