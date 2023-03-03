## Problem Highlights

* 🔗 **Leetcode Link:** [Merge Intervals](https://leetcode.com/problems/merge-intervals/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, Sliding Window
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What is the minimum interval length?
    - The minimum interval length is 1.
- Are the intervals in sorted order?
    - No the intervals are not guaranteed to be sorted order.
- What is my time and space complexity?
    - O(nlogn) time and O(1) space not including the resulting output array.
   
```markdown
HAPPY CASE
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.

EDGE CASE
Input: intervals = [[1,5]]
Output: [[1,5]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Yes this very helpful. We need to sort the intervals so that we can use the sliding window technique to merge the overlapping intervals.
- Two pointer solutions (left and right pointer variables)
    - Not very useful here.
- Storing the elements of the array in a HashMap or a Set
    - Also not very useful here.
- Traversing the array with a sliding window
    - Yes, we are merging overlapping intervals and need a sliding window to extend the overlapping interval for output.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will match the previous high against each low. If previous high is higher than low, then extend the sliding window and merge, otherwise close the window. 

```markdown
1. Go through all intervals in sorted order
2. Store first interval into results to start sliding window
3. Check high in previous sliding window against current low
    a. If previous high is higher than low, then extend the sliding window and merge
    b. Otherwise close the window
4. Return the results
```

⚠️ **Common Mistakes**

* Remember to ask and clarify about the inputs and outputs. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        result = []
        # Go through all intervals in sorted order
        for low, high in sorted(intervals):
            # Store first interval into results to start sliding window
            if not result:
                result.append([low, high])

            # Check high in previous sliding window against current low
            previousHigh = result[-1][1]
            if previousHigh >= low:
                # If previous high is higher than low, then extend the sliding window and merge
                result[-1][1] = max(previousHigh, high)
            else:
                # Otherwise close the window
                result.append([low,high])

        # Return the results
        return result
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of intervals.

* **Time Complexity**: `O(NlogN)`, we need to sort the intervals before using the sliding window technique.
* **Space Complexity**: `O(1)`, not including the resulting output array.