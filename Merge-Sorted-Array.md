## Problem Highlights

* 🔗 **Leetcode Link:** [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/), [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/), [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input array be empty?
    - Yes, that is possible
- What is the space and time complexity?
    - We want O(n) time and O(1) space. 

```markdown
HAPPY CASE
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].

EDGE CASE
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The arrays are already sorted.
- Two pointer solutions (left and right pointer variables). 
    - We can start at index m - 1 for nums1 and index n -1 for nums2, find the larger number and start inserting at m + n - 1 index of nums1. Repeat until we reach index 0 for nums2.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) space. But the interviewer wants to solve this problem in O(1) space.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can start at index m - 1 for nums1 and index n -1 for nums2, find the larger number and start inserting at m + n - 1 index of nums1. Repeat until we reach index 0 for nums2.


```markdown
1. Initialize three pointers
    a.point at last number in nums1
    b.point at the last number of nums2
    c. point at the last index in nums1
2) While nums2 has numbers to merge
    a) Get the larger number and insert into the insertIndex of nums1
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        # Initialize three pointers
            # a.point at last number in nums1
            # b.point at the last number of nums2
            # c. point at the last index in nums1
        index1, index2, insertIndex = m - 1, n - 1, m + n - 1

        # While nums2 has numbers to merge
        while index2 >= 0:
            # Get the larger number and insert into the insertIndex of nums1
            if index1 >=0 and nums1[index1] > nums2[index2]:
                nums1[insertIndex] = nums1[index1]
                index1 -= 1
            else:
                nums1[insertIndex] = nums2[index2]
                index2 -= 1

            insertIndex -= 1
```
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // Initialize three pointers
            // a.point at last number in nums1
            // b.point at the last number of nums2
            // c. point at the last index in nums1
        int tail1 = m - 1, tail2 = n - 1, insertIndex = m + n - 1;

        // While nums2 has numbers to merge
        while (tail2 >= 0) {
            if (tail1 >= 0 && nums1[tail1] > nums2[tail2]) {
                // Get the larger number and insert into the insertIndex of nums1
                nums1[insertIndex] = nums1[tail1];
                tail1--;
            } else {
                nums1[insertIndex] = nums2[tail2];
                tail2--;
            }
            insertIndex--;
        }
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array nums1.
Assume `M` represents the number of items in the array nums2.

* **Time Complexity**: `O(N + M)` because we need to traverse all numbers in both array.
* **Space Complexity**: `O(1)` because we only need three pointers to do the job.