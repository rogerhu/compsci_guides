## Problem Highlights

* 🔗 **Leetcode Link:** [Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
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
    - No, the number will begin at 1 thru n.

- Will the picked number alway be between 1 thru n?
    - Yes, the picked number is always between 1 thru n.

- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: n = 10, pick = 6
Output: 6

Input: n = 1, pick = 1
Output: 1

EDGE CASE
Input: n = 2, pick = 1
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The range of possible numbers is already sorted, we can use binary search on this.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not the number is in the upper half or lower half of the sorted range. 
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can distinguish between the smaller and larger half, where we can find our number. 


```markdown
1. Initialize left and right pointers
2. While left pointer is less than or equal to right pointer we have not exhausted the possible numbers
    a. Get the mid point of the two pointers 
    b. Check if mid point is less than, greater than, or equal to the picked number.
        i. If mid point number is greater than the picked number, then we need to check the smaller half for the picked number. Set the right pointer to mid pointer - 1
        ii. If mid point number is less than the picked number, then we need to check the larger half for the picked number. Set the left pointer to mid pointer + 1
        iii. If mid point number is equal to picked number, then return the picked number 
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        # Initialize left and right pointers
        l, r = 1, n

        # While left pointer is less than or equal to right pointer we have not exhausted the possible numbers
        while l <= r:
            # Get the mid point of the two pointers 
            mid = (l+r) // 2

            # Check if mid point is less than, greater than, or equal to the picked number.
            guessResult = guess(mid)
            match guessResult:

                # If mid point number is greater than the picked, then we need to check the smaller half for the picked number. Set the right pointer to mid pointer - 1
                case -1:
                    r = mid - 1

                # If mid point number is less than the picked, then we need to check the larger half for the picked number. Set the left pointer to mid pointer + 1
                case 1:
                    l = mid + 1
                
                # If mid point number is equal to picked number, then return the picked number
                case 0:
                    return mid
                    
                case _:
                    print("Error")
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