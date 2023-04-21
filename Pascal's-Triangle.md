## Problem Highlights

* 🔗 **Leetcode Link:** [Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion, DP, Bottom-Up
* 🗒️ **Similar Questions**: [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/), [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/), [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- At most, how many rows of the pascal triangle do we generate?
    - 30
- What are the minimum row?
    - 1 row, there is no need to account for edges case of zero row.
- What is the time and space complexity for this problem?
    - `O(n^2)` time and `O(n^2)` space will do. 


Run through a set of example cases:

```markdown
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```
![Image1](https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif)
```markdown
Input: numRows = 6
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1], [1,5,10,10,5,1]]

EDGE CASE 
Input: numRows = 1
Output: [[1]]
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- 2D Array Top-Left -> Bottom-Right, Bottom-Up DP Technique
    - We are not working with a 2D Array, but we can use the Bottom-Up DP Technique to build up our answers from previous calculation.
- Knapsack-Type Approach
    - This does not fit the knapsack-type approach.
- Cache previous results, generally
    - Yes, we can use the Top-Down DP Technique and recursion to build our answers from previous recursive calls, but that would be a round about way of solving this algorithm.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Bottom-Up DP Technique, we will build up our answer. We will find the next row of the pascal triangle from the previous row. We can add zeros to the beginning and end of row to help with calculation of next row.

```markdown
1. Set the basecase, [1] in results
2. Generate number of rows as requested
    a. Add zeros to the beginning and end of previous row for calculation
    b. For each item in row, set it to be itself plus the next number.
    c. Remove the last zero, because we only need one new number
    d. Append the new row to results.
7. Return the results
```

⚠️ **Common Mistakes**

* This problem may looks quite simple at the first glance, however we are using the fundamentals of dynamic programming to record past results to be reused in future calculation.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        # Set the basecase, [1] in results
        results = [[1]]

        # Generate number of rows as requested
        for i in range(1, numRows):
            # Add zeros to the beginning and end of previous row for calculation
            row = [0] + results[-1] + [0]

            # For each item in row, set it to be itself plus the next number
            for j in range(len(row) - 1):
                row[j] = row[j] + row[j + 1]

            # Remove the last zero, because we only need one new number
            row.pop()
            # Append the new row to results
            results.append(row)
        
        # Return the results
        return results
```
```java
class Solution {
  public List<List<Integer>> generate(int numRows) {
    List<List<Integer>> allrows = new ArrayList<List<Integer>>();
    ArrayList<Integer> row = new ArrayList<Integer>();
    
    // Generate number of rows as requested
    for(int i=0;i<numRows;i++) {
      // Add zeros to the beginning and end of previous row for calculation
      row.add(0, 1);

      // For each item in row, set it to be itself plus the next number
      for(int j=1;j<row.size()-1;j++)
        row.set(j, row.get(j)+row.get(j+1));

      allrows.add(new ArrayList<Integer>(row));
    }

    // Return the results
    return allrows;
  }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of rows

* **Time Complexity**: `O(N^2)`, because we need to build up each row, and the number of items in each rows averages `N/2` items. 
* **Space Complexity**: `O(N^2)`, because we need to create the results.
