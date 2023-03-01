## Problem Highlights

* 🔗 **Leetcode Link:** [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Reverse String](https://leetcode.com/problems/reverse-string/), [Reverse String II](https://leetcode.com/problems/reverse-string-ii/), [Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input parameter be Null?
  - Let’s assume no input will be Null. The minimum input has two numbers.

- What is the time and space complexity requirements?
    - O(n) time and O(1) space complexity. (The output array does not count as extra space for space complexity analysis.)


```markdown
HAPPY CASE
Input: nums = [1,2,3,4]
Output: [24,12,8,6]

Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]

EDGE CASE
Input: nums = [-3,3]
Output: [-9,-9]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?

- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us.

- Storing the elements of the array in a HashMap or a Set
    - In reversing, hashing elements and storing them may not yield an optimal solution.

- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use two pointer from both ends of the array to generate the product from both sides of each number to achieve the product except self. 

```markdown
1) Create product from the left side of each num and store in output array. 
    a) Store product into output array
    b) Multiply product with the current number to develop left product
2) Create product from the right side of each num and multiply with the left product stored in output array.
    a) Multiply right product with the left product stored in output array
    b) Mutiply product with the current number to develop right product
3) Return the output array
```

⚠️ **Common Mistakes**

* You may have a hard time understanding this problem because it seems to be outside of the 4 common techniques for strings/arrays

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        # Create product from the left side of each num and store in output array.
        leftProduct = 1
        output = [1] * len(nums)
        for i in range(len(nums)):
            # Store product into output array
            output[i] = leftProduct
            # Multiply product with the current number to develop left product
            leftProduct *= nums[i]
        
        # Create product from the right side of each num and multiply with the left product stored in output array.
        rightProduct = 1
        for j in range(len(nums) - 1,-1,-1):
            # Multiply right product with the left product stored in output array.
            output[j] *= rightProduct
            # Mutiply product with the current number to develop right product
            rightProduct *= nums[j]
        
        # Return the output array
        return output
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of items in the array.


* **Time Complexity**: O(n), traversing done on every number of the array twice.
* 
* **Space Complexity**: O(1), when the output array does not count as extra space for space complexity analysis.