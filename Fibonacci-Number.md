## Problem Highlights

* 🔗 **Leetcode Link:** [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion, DP, Top-Down, Bottom-Up
* 🗒️ **Similar Questions**: [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Super Pow](https://leetcode.com/problems/super-pow/),  [Pow(x.n)](https://leetcode.com/problems/powx-n/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What is the maximum fibonacci sequence?
    - 30
- What are the minimum fibonacci sequence?
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

**Solution 1:**

**General Idea:** Bottom-Up DP Technique, we will build up our answer. 

```markdown
1. Create a array to store fibonacci numbers to build upon
    a. Set the given numbers fibonacci at 0 is 0 and fibonacci at 1 is 1.
2. From the given numbers we can build up to any number of fibonacci.
    a. Fibonacci is the result of the addition of the two previous numbers.
    b. Append such numbers to array until array has reached target number
4. Return the target fibonacci number stored in our array
```

**Solution 2:**

**General Idea:** Top-Down DP Technique, we will recursively build up to our answer. 

```markdown
1. Create helper function to process fibonacci numbers
    a. Basecase: Fibonacci number was found in our cache
    b. Recursively call for the answer to our target fibonacci number
2.  Create cache by using dictionary to store past result for quick look up when recursively asking for answer to the target fibonacci number
3. Call our helper function and return the answer 
```

**Solution 3:**

**General Idea:** Top-Down DP Technique, we will recursively build up to our answer using a functool called @cache to store our past results from recursive calls used to calculate our target fibonacci nunber.

```markdown
1. Declare that we would like to cache for our function
2. Set basecase 0 and 1
3. Return the recursive call to obtain our target fibonacci number 
```

**Solution 4:**

**General Idea:** Bottom-Up DP Technique, we will build up our answer.(Using `O(1)` space) 

```markdown
0. Set basecase if target is 0 or 1 return n
1. Create a variable for fibonacci numbers to build upon
    a. Set the given numbers fibonacci at n-2 to be 0 and fibonacci at n-1 to be 1.
2. From the given numbers we can build up to any number of fibonacci.
    a. Fibonacci is the result of the addition of the two previous numbers.
    b. Set n-2 to be n-1 and set n-1 to be n-2 + n-1 
3. Return the n-2 + n-1 as the target fibonacci number
```

⚠️ **Common Mistakes**

* This problem looks quite simple at the first glance, however requires considering the need to recalculate fibonacci n-1 and fibonacci n-2 at recursive call. For example the 5th fibonacci will need to repeat calculation for at 4, 3, 2, 1.  We need to use @cache or create a dictionary to store pass results in order to meet `O(n)` runtime expectations. Otherwise it will be `O(n!)`.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Solution 1:**

```python
class Solution:
    def fib(self, n: int) -> int:
        # Create a array to store fibonacci numbers to build upon
        # Set the given numbers fibonacci at 0 is 0 and fibonacci at 1 is 1.
        fib = [0,1]

        # From the given numbers we can build up to any number of fibonacci
        for i in range(2, n+1):
            # Fibonacci is the result of the addition of the two previous numbers
            # Append such numbers to array until array has reached target number
            fib.append(fib[i-1] + fib[i-2])
        
        # Return the target number stored in our array
        return fib[n]
```

**Solution 2:**

```python
class Solution:
    def fib(self, n: int) -> int:
        # Create helper function to process fibonacci numbers
        def helper(i) -> int:
            # Basecase: Fibonacci number was found in our cache
            if i in fibMemo:
                return fibMemo[i]

            # Recursively call for the answer to our target fibonacci number
            return helper(i-1) + helper(i-2)

        # Create cache by using dictionary to store past result for quick look up when recursively asking for answer to the target fibonacci number
        fibMemo = {0:0, 1:1}
        
        # Call our helper function and return the answer 
        return helper(n)
```

**Solution 3:**

```python
class Solution:
    @cache
    def fib(self, n: int) -> int:
        if n == 0 or n == 1:
            return n
            
        return self.fib(n-1) + self.fib(n-2)
```

**Solution 4:**

```python
class Solution:
    def fib(self, n: int) -> int:
        # Set basecase if target is 0 or 1 return n
        if n == 0 or n == 1:
            return n

        # Create a variable for fibonacci numbers to build upon
        # Set the given numbers fibonacci at n-2 to be 0 and fibonacci at n-1 to be 1
        n_minus_two, n_minus_one = 0, 1

        # From the given numbers we can build up to any number of fibonacci
        # Fibonacci is the result of the addition of the two previous numbers
        for i in range(2, n):
            # Set n-2 to be n-1 and set n-1 to be n-2 + n-1
            n_minus_two, n_minus_one = n_minus_one, n_minus_two + n_minus_one
        
        # Return the n-2 + n-1 as the target fibonacci number
        return n_minus_two + n_minus_one
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the target fibonacci number

* **Time Complexity**: `O(N)`, because we need to dig down or build up to the `N` fibonacci number and require n-1 and n-2 fibonacci numbers of which require n-1 and n-2 fibonacci, until the base case.
* **Space Complexity**: `O(N)`/`O(1)`, because for the Top-Down approach we need to store fibonacci numbers until `N`. However for the Bottom-Up approach we could push the space complexity to `O(1)` by reusing the n-2 and n-1 variables.
