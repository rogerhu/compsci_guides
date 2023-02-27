## Problem Highlights

* 🔗 **Leetcode Link:** [Pow(x.n)](https://leetcode.com/problems/powx-n/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Recursion, Divide and Conquer
* 🗒️ **Similar Questions**: [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Super Pow](https://leetcode.com/problems/super-pow/)
    
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
- What if either exponent is equal to the maximum value of integer or minimum value of integer?
    * When we have exponent as the minimum value of integer, the answer can be either 1 or 0. If the base is 1 or -1, having exponent as integer’s minimum value, will have answer as 1 otherwise 0. Similarly we can have only 1 with exponent as maximum value of integer, because of the constraint on the output is below 10000.


Run through a set of example cases:

```markdown
HAPPY CASE
Input: x = 2.00000, n = 10
Output: 1024.00000

Input: x = 2.10000, n = 3
Output: 9.26100

EDGE CASE 
Input: x = 2.00000, n = -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

We can implement fast modulo exponentiation either in a recursive manner or iteratively. In fast modulo exponentiation, we multiply the base in the power of 2. By this, we meant that we keep on multiplying the base by itself. So, in the first step, the base becomes squared of itself.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Convert exponent in binary format and keep on multiplying the bases as per the exponent binary format. If we just use the normal way of calculation, when face 1 to the power of 10000, the computation complexity is too high. Consider this way: if we want to get 2^10.
```markdown
2^10  = 2^4 * 2^4 *2^2
2^4 = 2^2*2^2
2^2 = 2*2
```

```markdown
1. Break into base case where n = 0 or n = 1 or n = -1
2. recurrent relations where n is even or odd numbers.
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering many corner cases. Two branch for computing pow(x, n) with divide and conquer approach. 
```markdown
When n is odd, pow(x, n) = x * pow(x, n-1)

When n is even, pow(x, n) = pow(x, n/2) * pow(x, n/2).

Note when x is equal with 1.0, we could do a optimization here.

Another corner case is when n is -2^31, -n will trigger a integer overflow.
```   
* We can do it in O(logn) time, think about the reduction operations in logn time. We can do it similar. We can use a recursive solution. This problem is not hard naturally, but the O(logn) solution requires a bit more effort. Do consider recursion when you are found you can divide the problem into smaller problems.



## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def myPow(self, x, n):
       # Break into base case where n = 0 or n = 1 or n = -1
        if n == 0:
            return 1
        if n == 1:
            return x
        if n == -1:
            return 1 / x
        
        # recurrent relations where n is even or odd numbers.
        result = self.myPow(x, n // 2)

        return result * result * (x if n % 2 else 1)

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(logN), because we used fast modulo exponentiation that takes logarithmic time to compute the exponent over a base. We can also intuitively find the time complexity. Since we have divided the exponent until it became 0. Thus the time required is dependent on logN, where N is the exponent.
* **Space Complexity**: O(1), since we have used a single variable to store the answer, the space complexity is constant.
