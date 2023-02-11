## Problem Highlights

* 🔗 **Leetcode Link:** [Find Subsequence of Length K With the Largest Sum](https://leetcode.com/problems/find-subsequence-of-length-k-with-the-largest-sum/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Sorted Range, Binary Search 
* 🗒️ **Similar Questions**: [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/), [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/), [Two Sum](https://github.com/codepath/compsci_guides/wiki/Two-Sum), [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be at least one number?
    - Yes, the array contains at least one number

- Will the number of selected numbers be less than total numbers?
    - Yes, the number of selected numbers will be less than total numbers.

- What is the space and time complexity?
    - We want O(nlogn) run time and O(n) space complexity. 


```markdown
HAPPY CASE
Input: nums = [2,1,3,3], k = 2
Output: [3,3]
Explanation:
The subsequence has the largest sum of 3 + 3 = 6.


Input: nums = [-1,-2,3,4], k = 3
Output: [-1,3,4]
Explanation: 
The subsequence has the largest sum of -1 + 3 + 4 = 6.

EDGE CASE
Input: nums = [-199], k = 1
Output: [-199]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - Sorting will be helpful here. We want to get the largest items and select those index.
- Two pointer solutions (left and right pointer variables). 
    - A two pointer will not help here. Considering the number of variations in subsequences O(n^n) runtime.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap is necessary for us to select the largest items and select those indexes. 
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this may seem like a backtracking problem, but backtracking will be too slow. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Collect the numbers in index pairs. Select the largest k numbers. Return the k largest numbers as sorted by index.  

```markdown
1. Hashmap/Tuple number in sorted index pairs. 
2. Select the largest k numbers
3. Sort largest k numbers by index
4. Return the k largest numbers as sorted by index.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def maxSubsequence(self, nums: List[int], k: int) -> List[int]:
        # Hashmap/Tuple number in sorted index pairs. 
        sortedNumAndIndexByNum = [[num,i] for i, num in enumerate(nums)]
        sortedNumAndIndexByNum.sort(key=lambda x:x[0])

        # Select the largest k numbers
        sortedSubsequenceByIndex = sortedNumAndIndexByNum[len(nums)-k:]

        # Sort largest k numbers by index
        sortedSubsequenceByIndex.sort(key=lambda x:x[1])

        # Return the k largest numbers as sorted by index.
        return [numAndIndex[0] for numAndIndex in sortedSubsequenceByIndex]
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(NlogN)` because we needed to use sort to collect the numbers ascending by value and then by index.
* **Space Complexity**: `O(N)` because we needed to create a hashmap/tuple pair of numbers and index. 