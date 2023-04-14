## Problem Highlights

* 🔗 **Leetcode Link:** [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion
* 🗒️ **Similar Questions**: [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Super Pow](https://leetcode.com/problems/super-pow/),  [Pow(x.n)](https://leetcode.com/problems/powx-n/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What are the maximum number of steps?
    - 45 steps
- What are the minimum number of steps?
    - 1 step
- What is the time and space complexity for this problem?
    - `O(n)` time and `O(n)` space will do. 


Run through a set of example cases:

```markdown
HAPPY CASE
Input: n = 2
Output: 2
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Input: n = 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

EDGE CASE 
Input: n = 1
Output: 1
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For problems outside of the main catagories, we want to consider the input and output:

We are working with a `n` steps to climb and the ability to take one step or two steps. And the number of ways to climb it. Let's start with the premise there is 1 way to climb 1 step and 2 ways to climb 2 steps. How do we climb 3 steps? We have to be at step 1 or step 2, to get to step 3. So lets take 2 steps from step 1 and 1 step from step 2, which is equalivent to adding the number of ways from step 1 and step 2. A total of 3 ways to climb 3 steps. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will recursively obtain the number of ways to traverse `n-1` steps and `n-2` steps before `n` steps. Then add the number of ways to get `n` steps.

```markdown
1. Store the step that we have already calculated for quick access.
1. Set the base case at n = 1 and n = 2 
2. Return the recursive step n-1 + recursive step n-2, at each step. 
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering the need to recalculate step n-1 and step n-2 at each n. We need to use @cache or create a dictionary to store pass results in order to meet `O(n)` runtime expectations. Otherwise it will be `O(n!)`, because for example 5 will need to repeat calculation for at 4, 3, 2, 1. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

Auto Cache

```python
class Solution:
    # Store the step that we have already calculated for quick access.
    @cache
    def climbStairs(self, n: int) -> int:
        # Set the base case at n = 1 and n = 2 
        if n == 1:
            return 1
        if n == 2:
            return 2

        # Return the recursive step n-1 + recursive step n-2, at each step. 
        return self.climbStairs(n-1) + self.climbStairs(n-2)
```

Manual Cache
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # Store the step that we have already calculated for quick access.
        memo ={}
        # Set the base case at n = 1 and n = 2
        memo[1] = 1
        memo[2] = 2
        
        # Return the recursive step n-1 + recursive step n-2, at each step. 
        def climb(n: int) -> int:
            if n in memo:
                return memo[n]
            else:
                memo[n] = climb(n-1) + climb(n-2)
                return memo[n]
            
        return climb(n)
```

    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the steps needed to climb

* **Time Complexity**: `O(N)`, because we need to dig down `N` steps to obtain n-1 and n-2 steps for calculations
* **Space Complexity**: `O(N)`, because we need to store the number of way to traverse a step at each step.
