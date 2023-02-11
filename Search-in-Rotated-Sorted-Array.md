## Problem Highlights

* 🔗 **Leetcode Link:** [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Sorted Range, Binary Search 
* 🗒️ **Similar Questions**: [Binary Search](https://leetcode.com/problems/binary-search/), [First Bad Version](https://leetcode.com/problems/first-bad-version/), [Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be negative numbers?
    - Yes, the number will be both negative and positive

- Will there alway be a pivot point?
    - Yes, there will always be a pivot point

- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

EDGE CASE
Input: nums = [1], target = 0
Output: -1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The range of possible numbers is semi-sorted, we can run binary search on the two halves. A full sort would exceed our runtime expectations.
- Two pointer solutions (left and right pointer variables). 
    - First we split the problem into two sorted arrays. Then we can use binary search on both sorted havles. 
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Let's split the array into two sorted arrays by finding the inflexion point using binary search. I.E. [[Find Minimum in Rotated Sorted Array]]. We can then run binary search on the two sorted arrays using binary search. I.E. [[Binary Search Problem]]


```markdown
1. Create a function to get the inflexion point, the minimum value in list. For more details see [[Find Minimum in Rotated Sorted Array]]
2. Create/Run binary search for target in left and right half. For more details see [[Binary Search Problem]]
3. Return the search results. The max function checks between a positive value(found index) vs negative value(not found index).
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        # Create/Use a function to get the inflexion point, the minimum value in list. For more details see [[Find Minimum in Rotated Sorted Array]]
        minIndex = self.findMinIndex(nums)

        # Create/Run binary search for target in left and right half. For more details see [[Binary Search Problem]]
        checkLeftHalf = self.binarySearch(nums, 0, minIndex, target)
        checkRightHalf = self.binarySearch(nums, minIndex, len(nums) - 1, target)

        # Return the search results. The max function checks between a positive value(found index) vs negative value(not found index).
        return max(checkLeftHalf, checkRightHalf)

    # Create/Run binary search for target in left and right half. For more details see [[Binary Search Problem]]
    def binarySearch(self, nums: List[int], left: int, right: int, target: int) -> int:
        l, r = left, right

        while l <= r:
            mid = (l+r) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                l = mid + 1
            else:
                r = mid - 1
            
        return -1 

    # Create/Use a function to get the inflexion point, the minimum value in list. For more details see [[Find Minimum in Rotated Sorted Array]]
    def findMinIndex(self, nums: List[int]) -> int:
        l, r = 0, len(nums) - 1

        while l < r:
            mid = (l+r) // 2
            if nums[mid] > nums[r]:
                l = mid + 1
            else:
                r = mid
        
        return r
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(logN)` because we can used binary search for the inflexion point, then binary search on both sorted halves. 
* **Space Complexity**: `O(1)` because we only need two pointers to do the job.