## Problem Highlights

* 🔗 **Leetcode Link:** [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/), [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be an empty array?
    - No, you may assume that there is at least one numbber in the array

- What is the time and space complexity?
    - Squaring each element and sorting the new array is very trivial, could you find an O(n) time solution using a different approach?


```markdown
HAPPY CASE
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: After squaring, the array becomes [16,1,0,9,100].
After sorting, it becomes [0,1,9,16,100].

Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]

EDGE CASE (Empty Input)
Input: []
Output: [] 
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?

- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us, if we start at the two ends, we can get the largest absolute numbers and pair our way down to the smallest absolute number. We can reverse the new array afterwards to return.

- Storing the elements of the array in a HashMap or a Set
    - A hashset will be not helpful here.

- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Initialize two pointers left and right. Shift the left and right pointer towards each other and get the larger absolute number of the two, square, and add to the result list.


```markdown
1) Initialize left and right pointers
2) Initialize resultant array
3) While left is less than or equal right
    a) If left is larger, square it and add it to the resultant array, and shift left forward
    b) Else right is larger, square it, add it to the resultant array, and shift right forward.
4) Return the reversed resultant array
```

**General Idea:** Generate a new sorted list of squares


```markdown
1) For each element in the input, replace that index with the squared number
2) Sort the array
3) Return the sorted array
```

⚠️ **Common Mistakes**

* Remember to use two pointer to achieve O(N) time. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        # Initialize left and right pointers
        left, right = 0, len(nums) - 1
        # Initialize resultant array
        res = []

        # While left is less than or equal right
        while left <= right:
            # If left is larger, square it and add it to the resultant array, and shift left forward
            if abs(nums[left]) > abs(nums[right]):
                res.append(nums[left] ** 2)
                left += 1
            # Else right is larger, square it, add it to the resultant array, and shift right forward.
            else:
                res.append(nums[right] ** 2)
                right -= 1
                
        # Return the reversed resultant array
        return reversed(res)
```

```python
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        if not A:
            return A
        # For each element in the input, replace that index with the squared number
        for i in range(len(A)):
            A[i] = A[i]**2
        # Sort the array
        A.sort()
        # Return the sorted array
        return A
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.


* **Time Complexity**: O(n), we need to visit every item in the array to create squares of a sorted array.
* **Space Complexity**: O(n), we will need `N` space to create a sorted array. 
    * Do note O(1) space is possible with a squaring of each number and sorting, but this results in O(nlogn) time.