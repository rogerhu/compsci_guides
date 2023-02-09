## Problem Highlights

* 🔗 **Leetcode Link:** [Sqrt(x)](https://leetcode.com/problems/sqrtx/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Sorted Range, Binary Search 
* 🗒️ **Similar Questions**: [Binary Search](https://leetcode.com/problems/binary-search/), [First Bad Version](https://leetcode.com/problems/first-bad-version/), [Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be negative numbers?
    - No, the number is always positive 

- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: x = 4
Output: 2
Explanation: The square root of 4 is 2, so we return 2.

Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.

CASE
Input: n = 0
Output: 0
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The range of possible numbers are already sorted.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not the number is too large or too small so that we can eliminate half the number of possible values.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can decide whether or not the number exist in the left half or right half, with each iteration until we find our number or we exhaust our list. Binary Search


```markdown
1. Initialize left and right pointers
2. While left pointer is less than right pointer we have not exhausted the possible numbers
    a. Get the mid point of the two pointers 
    b. Check if mid point is less than or greater than our target
        i. If mid point is equal to our target then return our mid point
        ii. If mid point is less than our target, then we move the left pointer up to mid point + 1, because everything left of the mid point would be even further away from our target.
        iii. If mid point is greater than our target, then we move the right pointer down to mid point - 1, because everything to the right of mid point is invalid. 
3. Return the right pointer for the closes number to square for our target as it is the last remaining valid number.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def mySqrt(self, x: int) -> int:
        # Initialize left and right pointers
        l,r = 0, x
        # While left pointer is less than right pointer we have not exhausted the possible numbers
        while l <= r:
            # Get the mid point of the two pointers 
            mid = (l + r) // 2

            # Check if mid point is less than or greater than or equal to our target
            if mid ** 2 == x:
                # If mid point is equal to our target then return our mid point
                return mid
            
            elif mid ** 2 < x:
                # If mid point is less than our target, then we move the left pointer up to mid point + 1, because everything left of the mid point would be even further away from our target
                l = mid + 1
            else:
                # If mid point is greater than our target, then we move the right pointer down to mid point - 1, because everything to the right of mid point is invalid. 
                r = mid - 1
                
        # Return the right pointer for the closes number to square for our target as it is the last remaining valid number.
        return r
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(logN)` because we can eliminate half the possible versions with each check.
* **Space Complexity**: `O(1)` because we only need two pointers to do the job.