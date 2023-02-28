## Problem Highlights

* 🔗 **Leetcode Link:** [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion
* 🗒️ **Similar Questions**: [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Super Pow](https://leetcode.com/problems/super-pow/),  [Pow(x.n)](https://leetcode.com/problems/powx-n/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What are the maximum number of fibonacci-number?
    - 30
- What are the minimum number of fibonacci-number?
    - 0
- What is the time and space complexity for this problem?
    - `O(n)` time and `O(n)` space will do. 


Run through a set of example cases:

```markdown
HAPPY CASE
Input: n = 2
Output: 1
Explanation: F(2) = F(1) + F(0) = 1 + 0 = 1.

Input: n = 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

EDGE CASE 
Input: n = 0
Output: 0

Input: n = 1
Output: 1
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For problems outside of the main catagories, we want to consider the input and output:

We are working with a `n` number which is calculated by adding `n-1` number and `n-2` number. Each and every `n` number is calculated the same way. Recursion will work perfectly.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will recursively obtain `n-1` and `n-2` steps before add the numbers together.

```markdown
1. Store the step that we have already calculated for quick access.
1. Set the base case at n = 0 and n = 1 
2. Return the recursive step n-1 + recursive step n-2, at each step. 
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering the need to recalculate  n-1 and n-2 at each n. We need to use @cache or create a dictionary to store pass results in order to meet `O(n)` runtime expectations. Otherwise it will be `O(n!)`, because for example 5 will need to repeat calculation for at 4, 3, 2, 1. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    # Store the step that we have already calculated for quick access.
    @cache
    def fib(self, n: int) -> int:
        # Set the base case at n = 0 and n = 1 
        if n == 0:
            return 0
        if n == 1:
            return 1
        
        # Return the recursive step n-1 + recursive step n-2, at each step. 
        return self.fib(n-1) + self.fib(n-2)
```
```python
class Solution:
    # Store the step that we have already calculated for quick access.
    memo = {}
    memo[0] = 0
    memo[1] = 1

    def fib(self, n: int) -> int:
        # Set the base case at n = 0 and n = 1 
        if n in self.memo:
            return self.memo[n]
        else:
            self.memo[n] = self.fib(n-1) + self.fib(n-2)
            return self.memo[n]
```

    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the steps needed to climb

* **Time Complexity**: `O(N)`, because we need to dig down `N` steps to obtain n-1 and n-2  for calculations
* **Space Complexity**: `O(N)`, because we need to store the number of way to traverse a step at each step.
