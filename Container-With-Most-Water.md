## Problem Highlights

* 🔗 **Leetcode Link:** [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/), [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is it possible for us to have negative heights?
    - No, the lowest height possible in this problem is 0.
- What is the minimum number of heights?
    - The minimum number of heights is 2.
- What is the time and space complexity considerations?
    - Lets try for a runtime of O(n) and space of O(1)
   
```markdown
HAPPY CASE
Input: [1,8,6,2,5,4,8,3,7]
Output: 49 (The max area is between index 1 and 8 with a 
minimum height of 7, giving us a container of 7 by 7, which is 49)

Input: [1,1]
Output: 1

EDGE CASE
Input:[0,0]
Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - The array should not to be altered, the index of each height represents the width of the rectangle, which is important for us in determining the maximum area of a rectangle.
- Two pointer solutions (left and right pointer variables): 
    - Intuition, the largest possible area is the max width by max height. We know the max width, but we do not know the max height.If we start at the max width and reduce the width while maintaining the max height for each width, then we can get find the maximum area at each width.Using the left and right pointer we can determine every possible width of the rectangle.And we can maintain the maximum height at each width, by progressing the pointer with the lower height.
- Storing the elements of the array in a HashMap or a Set
    - Sort values into a hashmap might not be very useful here
- Traversing the array with a sliding window
    - A sliding window is useful for selecting a window, but that is not what we are trying to do here.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use the two pointer solution, we start at the max width and reduce the width while maintaining the max height for each width, then we can find the maximum area at each width. 

```markdown
1. Create largest range, by creating a left and right pointer
2. At every part of the range calculate the current area at this width and check against our max area
    a. Reduce the range/width while maintaining the maximum height
3. Return max area found
```

⚠️ **Common Mistakes**

* A less optimized approached would be to check every height against every other height. A n^2 approach.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # Create largest range, by creating a left and right pointer
        l, r = 0, len(height) - 1
        maxArea = 0

        # At every part of the range calculate the current area at this width and check against our max area
        while l < r:
            width = r - l
            minHeight = min(height[l], height[r])
            currArea = minHeight * width
            maxArea = max(maxArea, currArea)

            # Reduce the range/width while maintaining the maximum height 
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1

        # Return max area found
        return maxArea
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of heights in array.

* **Time Complexity**: `O(N)`, because we need to check the entire range of heights once. 
* **Space Complexity**: `O(1)`, because we only use a constant number of variable to hold our area information.