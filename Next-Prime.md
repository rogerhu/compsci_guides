# Problem

Given a number, return the next smallest prime number

Note: A prime number is greater than one and has no other factors other than 1 and itself.

## Problem Highlights

* 🔗 **Leetcode Link:** [N/A](https://www.geeksforgeeks.org/program-to-find-the-next-prime-number/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Math, Recursion
* 🗒️ **Similar Questions**: [Count Primes](https://leetcode.com/problems/count-primes/), [Ugly Number](https://leetcode.com/problems/ugly-number/), [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Is there guaranteed to be a solution?
  - Yes!
- Is the input number guaranteed to be positive?
  - No! This will cause edge case catching important!
- What makes a number prime? What does this number have to satisfy?
  - HINT: A prime number only has factors of itself and 1. This means 2, 3, 4, 5 … N-1 are not factors of this number

Run through a set of example cases:
   
```markdown
HAPPY CASE
Input: 5
Output: 7

Input: 1
Output: 2

EDGE CASE:
Input: -10
Output: 2
The smallest prime number is 2, so any input less than 2 should return 2
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort: Can we split a chunk? If we know the underlying strategy on when to split a chunk (which relies on the sorted version of the data), sorting may potentially help us.
- Two pointer solutions (left and right pointer variables): What we can do is take two pointer variables: start and end pointers. Then, point them with the two ends of the input string. When left pointer and right pointer pointing at different valid character, we should exit and return false immediately. Two pointer may help us reference both sides of the array at the same time. However, since the array is not sorted, a two pointer solution may not assist us as much.
- Storing the elements of the array in a HashMap or a Set
- Traversing the array with a sliding window. In a sliding window, the two pointers usually move in the same direction will never overtake each other. This ensures that each value is only visited at most twice and the time complexity is still O(n).
- Traversing the array twice/thrice (as long as fewer than n times) is still O(n). Sometimes traversing the array more than once can help you solve the problem while keeping the time complexity to O(n).
- This problem may be hard to match to a specific problem pattern since it relies on general math intuition.
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Infinitely loop above the input value and check every value after that if it is prime or not.

```markdown
1) Iterate from N+1 to INF
2) If the current number is prime, return this number
3) Else, continue to increment and re-evaluate the next number
4) Repeat until we find the next largest prime number, since it is guaranteed to exist
```

⚠️ **Common Mistakes**

* Watch out for edges cases such as:

- Negative Case, Example: `next_prime(-10) --> 2`
- Zero Case, Example: `next_prime(0) --> 2`
- What is the time complexity of your plan? Is it O(n)? There is actually an even optimal solution with a time complexity of O(√n)!
- What the largest factor of any number? Think of a few examples and write out the factors of those numbers.
    - Given a number n, the largest factor of n is √n. This should imply, the loop to find if a number is prime should stop at √n.
With this optimization, the is_prime(n) function will be O(√n).


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def next_prime(n):
    if n <= 1:
        return 2
        
    n += 1
    while True:
        if is_prime(n):
            return n
        n += 1

# Helper method
def is_prime(n):
    if n < 2:
        return False
    if n < 4:
        return True
    for i in range(4, int(math.sqrt(n))):
        if n % i == 0:
            return False
    return True
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(√n), where the overall time complexity of the problem depends largely on the input value. However, the best time complexity of `is_prime` is O(√n)
* **Space Complexity**: O(1), no data structures are needed in this problem