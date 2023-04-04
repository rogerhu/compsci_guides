## Problem Highlights

* 🔗 **Leetcode Link:** [3Sum](https://leetcode.com/problems/3sum/)
* 💡 **Difficulty:**  Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Number of Arithmetic Triplets](https://leetcode.com/problems/number-of-arithmetic-triplets/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is this question similar to [Two Sum](https://leetcode.com/articles/two-sum/) and [Two Sum II](https://leetcode.com/articles/two-sum-ii-input-array-is-sorted/)?
  - Yes, it's a good idea to take a first look at [Two Sum](https://leetcode.com/articles/two-sum/) and [Two Sum II](https://leetcode.com/articles/two-sum-ii-input-array-is-sorted/). Two Sum, Two Sum II and 3Sum share a similarity that the sum of elements must match the target exactly. A difference is that, instead of exactly one answer, we need to find all unique triplets that sum to zero.
- Can you modify the input array?
  - Yes, you can especially if you want to avoid copying it due to memory constraints.


```markdown
Example 1:
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]

Example 2:

Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.

Example 3:

Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - Sort the array so we can skip repeated values.
- Two pointer solutions (left and right pointer variables). 
    - We can start at index m - 1 for nums1 and index n -1 for nums2, find the larger number and start inserting at m + n - 1 index of nums1. Repeat until we reach index 0 for nums2.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown

```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.


* **Time Complexity**: 
* **Space Complexity**: 