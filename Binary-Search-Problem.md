## Problem Highlights

* 🔗 **Leetcode Link:** [Binary Search](https://leetcode.com/problems/binary-search/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Sorted Array, Binary Search 
* 🗒️ **Similar Questions**: [First Bad Version](https://leetcode.com/problems/first-bad-version), [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input array be empty?
    - No, there will alway be at least one number.

- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1

EDGE CASE
Input: nums = [-1], target = 1
Output: 0
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The arrays are already sorted.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not the number exist in the left half or right half, with each iteration we find our number or exhaust our list. 
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can decide whether or not the number exist in the left half or right half, with each iteration we find our number or exhaust our list. 


```markdown
1. Initialize left and right pointers
2. While left pointer is less than right pointer we have not exhausted the num list
    a. Get the mid point of the two pointers 
    b. Check if mid point is less than, greater than, or equal to target
        i.if mid point is less than target, then we know everything to the left of mid point can be eliminated from search
        ii. if mid point is greater than target, then we know everything to the right of mid point can be eliminated from search
        iii. otherwise the number is equal, so we return the mid index
3) The left pointer is greater than the right pointers, we have exhausted the num list, we cannot find the target, return -1
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # Initialize left and right pointers
        l, r = 0, len(nums) - 1
        
        # While left pointer is less than right pointer we have not exhausted the num list
        while l <= r:
            # Get the mid point of the two pointers 
            mid = (l + r) // 2
            
            # Check if mid point is less than, greater than, or equal to target

            # if mid point is less than target, then we know everything to the left of mid point can be eliminated from search
            if nums[mid] < target:
                l = mid + 1
            # else if mid point is greater than target, then we know everything to the right of mid point can be eliminated from search
            elif nums[mid] > target:
                r = mid - 1 
            # else the number is equal, so we return the mid index
            else:
                return mid
        
        # The left pointer is greater than the right pointers, we have exhausted the num list, return -1 
        return -1
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(logN)` because we can eliminate half the possible numbers with each check.
* **Space Complexity**: `O(1)` because we only need two pointers to do the job.