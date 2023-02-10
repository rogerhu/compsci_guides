## Problem Highlights

* 🔗 **Leetcode Link:** [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Sorted Range, Binary Search 
* 🗒️ **Similar Questions**: [Binary Search](https://leetcode.com/problems/binary-search/), [First Bad Version](https://leetcode.com/problems/first-bad-version/), [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be negative numbers?
    - Yes, the number will be both negative and positive

- Will the array alway be rotated?
    - Yes, the array will at least be rotated once. 

- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.

Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.

EDGE CASE
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The range of possible numbers are semi-sorted, we can use binary search on this. A full sort will require nlogn time and does not meet runtime expectations.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not we are in the smaller half of the array. Upon logn splits we will find the smallest value.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can distinguish between the smaller and larger half, with each iteration until we select the smaller half and exhaust our possiblities. Binary Search


```markdown
1. Initialize left and right pointers
2. While left pointer is less than right pointer we have not exhausted the possible numbers
    a. Get the mid point of the two pointers 
    b. Check if mid point is less than or greater than the right pointer
        i. If mid point is greater than the right pointer, then the smaller half is the right half. Set the left pointer to mid pointer + 1.
        ii. If mid pointer is less than the right pointer, then the smaller half is the left half. Set the right pointer to the mid pointer 
3. Return number at right pointer for smallest number in list 
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        # Initialize left and right pointers
        l, r = 0, len(nums) - 1

        # While left pointer is less than right pointer we have not exhausted the possible numbers
        while l < r:
            # Get the mid point of the two pointers 
            mid = (l + r) // 2

            # Check if mid point is less than or greater than the right pointer
            if nums[mid] > nums[r]:
                # If mid point is greater than the right pointer, then the smaller half is the right half. Set the left pointer to mid pointer + 1.
                l = mid + 1
            else:
                # If mid pointer is less than the right pointer, then the smaller half is the left half. Set the right pointer to the mid pointer
                r = mid
        
        # Return number at right pointer for smallest number in list 
        return nums[r]
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(logN)` because we can eliminate half the possible values with each check.
* **Space Complexity**: `O(1)` because we only need two pointers to do the job.