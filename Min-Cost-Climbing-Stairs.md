## Problem Highlights

* 🔗 **Leetcode Link:** [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion, DP, Bottom-Up
* 🗒️ **Similar Questions**: [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/), [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Super Pow](https://leetcode.com/problems/super-pow/),  [Pow(x.n)](https://leetcode.com/problems/powx-n/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What is the maximum number of steps?
    - 999
- What are the minimum number of steps?
    - 0
- What is the time and space complexity for this problem?
    - `O(n)` time and `O(1)` space will do. 


Run through a set of example cases:

```markdown
Input: cost = [10,15,20]
Output: 15
Explanation: You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.

Input: cost = [1,100,1,1,1,100,1,1,100,1]
Output: 6
Explanation: You will start at index 0.
- Pay 1 and climb two steps to reach index 2.
- Pay 1 and climb two steps to reach index 4.
- Pay 1 and climb two steps to reach index 6.
- Pay 1 and climb one step to reach index 7.
- Pay 1 and climb two steps to reach index 9.
- Pay 1 and climb one step to reach the top.
The total cost is 6.

EDGE CASE 
Input: cost = []
Output: 0
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- 2D Array Top-Left -> Bottom-Right, Bottom-Up DP Technique
    - We are not working with a 2D Array, but we can use the Bottom-Up DP Technique to build up our answers from previous calculation.
    
- Knapsack-Type Approach
    - This does not fit the knapsack-type approach.

- Cache previous results, generally
    - Yes, we can use the Top-Down DP Technique and recursion to build our answers from previous recursive calls.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Bottom-Up DP Technique, we will build up our answer. 

```markdown
1. Manage edge case of 2 steps and under
2. Reuse the cost array to build up to our answer
3. From the given numbers we can build up to any number of number of steps.
    a. The minimum cost of step is the result of the current step and minimum between two previous steps
4. Return the minimum cost of climbing stairs by getting the minimum cost of last two steps
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering the need to recalculate minCostClimbingStairs n-1 and minCostClimbingStairs n-2. It will be `O(n!)`. So use DP to bring down the cost to `O(n)`

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        # Manage edge case of 2 steps and under
        if len(cost) <= 2:
            return min(cost)
        
        # Reuse the cost array to build up to our answer
        for i in range(2, len(cost)):
            # From the given numbers we can build up to any number of number of steps
            # The minimum cost of step is the result of the current step and minimum between two previous steps
            cost[i] = cost[i] + min(cost[i-1], cost[i-2])
        
        # Return the minimum cost of climbing stairs by getting the minimum cost of last two steps
        return min(cost[i], cost[i-1])
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the target fibonacci number

* **Time Complexity**: `O(N)`, because we need to build up to the `N` steps
* **Space Complexity**: `O(1)`, because for the Bottom-Up approach we reuse the given array
