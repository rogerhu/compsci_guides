## Problem Highlights

* 🔗 **Leetcode Link:** [Power of Two](https://leetcode.com/problems/power-of-two/)
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

- How do we stimulate the operation of evaluating exponents?
    * An exponent can easily be evaluated by multiplying the base exponent number of times. So, we can easily simulate this using a loop.


Run through a set of example cases:

```markdown
HAPPY CASE
Input: n = 3
Output: false

Input: n = 16
Output: true
Explanation: 2^4 = 16

EDGE CASE 
Input: n = 0
Output: false

Input: n = 1
Output: true
Explanation: 2^0 = 1
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For problems outside of the main catagories, we want to consider the input and output:

We are working with a number and checking if it is a power of two. We know for sure any number is a power of two if it can be divided by 2 until it becomes 1, along the way down it must also be even. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will recursively divide the number by 2 until it either reaches 1 and return true or it is no longer even and return false. We also need to consider the edge case 0, which is false.

```markdown
1. Break into base case where n = 1 return True
2. Break into base case where n = 0 or n is not even return False
3. Recursively divide number by 2
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering the edge case 0 and 1. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        # Break into base case where n = 1 return True
        if n == 1:
            return True
            
        # Break into base case where n = 0 or n is not even return False
        if n % 2 != 0 or n == 0:
            return False
        
        # Recursively divide number by 2
        return self.isPowerOfTwo(n/2)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number given

* **Time Complexity**: O(logN), because we are dividing n by 2 each time, until it is one
* **Space Complexity**: O(1), the space complexity is constant if you do not count the recursive stack.
