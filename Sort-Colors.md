## Problem Highlights

* 🔗 **Leetcode Link:** [Sort Colors](https://leetcode.com/problems/sort-colors/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What is the time and space complexities for this problem?
    - O(n) time and O(1) space. In addition to O(n) time, one-pass. Do not use a second loop.
   
```markdown
HAPPY CASE
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

Input: nums = [2,0,1]
Output: [0,1,2]

EDGE CASE
Input: nums = [2,2,2,2,2,2]
Output: [2,2,2,2,2,2]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - We cannot use a sort here, because any general sorting algorithm has a time complexity of O(nlogn)
- Two pointer solutions (left and right pointer variables)
    -  What we can do is take three pointer variables: left,middle,right pointers. Then, swap all the 0 to left, 1 to the middle, and 2 to the right. 
- Storing the elements of the array in a HashMap or a Set
    - This will cost us O(n) space. 
- Traversing the array with a sliding window
    - We are not using one point and a variable to identify a window size, we want to sort.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** What we can do is take three pointer variables: left, middle, right pointers. Then, swap all the 0 to left, 1 to the middle, and 2 to the right. 

```markdown
1. Create left, middle, and right pointers.
2. While the middle pointer is less than or equal to right pointer, we have not visited each number and sorted the numbers
    a)If middle pointer is pointing at a 0, then swap with the left pointer and move both left and middle pointers forward. Because we setup the left pointer spot to be ready for another zero. Middle pointer can go forward to try next number, because we are garanteed to use spot for left pointer.
    b)If middle pointer is pointing at a 1, then move middle pointer forward. Because, the 1 is in the right place for the middle pointer. 
    c)If middle pointer is pointing at a 2, then swap with the right pointer and move the right pointer backwards. Because we setup the right pointer spot to be ready for another zero. Middle pointer needs to remain at the original spot because we cannot garanteed to that spot.
3. Return the sorted array
```

⚠️ **Common Mistakes**

* A less optimized approached would be to use a hashmap or a sort, but that will make this problem too easy.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # Create left, middle, and right pointers.
        l, mid, r = 0, 0, len(nums) - 1

        # While the middle pointer is less than or equal to right pointer, we have not visited each number and sorted the numbers
        while mid <= r:

            # If middle pointer is pointing at a 0, then swap with the left pointer and move both left and middle pointers forward. Because we setup the left pointer spot to be ready for another zero. Middle pointer can go forward to try next number, because we are garenteed to use spot for left pointer.
            if nums[mid] == 0:
                nums[l], nums[mid] = nums[mid], nums[l]
                l += 1
                mid += 1

            # If middle pointer is pointing at a 1, then move middle pointer forward. Because, the 1 is in the right place for the middle pointer. 
            elif nums[mid] == 1:
                mid += 1
            
            # If middle pointer is pointing at a 2, then swap with the right pointer and move the right pointer backwards. Because we setup the right pointer spot to be ready for another zero. Middle pointer needs to remain at the original spot because we cannot garanteed to that spot.
            else:
                nums[mid], nums[r] = nums[r], nums[mid]
                r -= 1
        
        return nums
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of numbers in nums array.

* **Time Complexity**: O(n), traversing the nums array is done in one loop
* **Space Complexity**: O(1), we simply used three pointers.