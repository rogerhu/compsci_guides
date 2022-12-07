## Problem Highlights

* 🔗 **Leetcode Link:** [Single Number](https://leetcode.com/problems/single-number/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Hashset, XOR
* 🗒️ **Similar Questions**: [Single Number II](https://leetcode.com/problems/single-number-ii/), [Single Number III](https://leetcode.com/problems/single-number-iii/), [Missing Number](https://leetcode.com/problems/missing-number/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be an array with all duplicates?
  - No, it is guaranteed that every element appears twice except for one. Find that single one.

- What is the time and space complexity?
    - You must implement a solution with a linear runtime complexity and use linear space.
        - As a bonus, you can implement a solution with a linear runtime complexity and constant space.

```markdown
HAPPY CASE
Input: nums = [2,2,1]
Output: 1

Input
Input: nums = [4,1,2,1,2]
Output: 4

EDGE CASE (Multiple Spaces)
Input: nums = [1]
Output: 1
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve our time complexity?
- Two pointer solutions (left and right pointer variables)
    - Does Two pointers help us find duplicates?
- Storing the elements of the array in a HashMap or a Set
    - A hashset can be used to count numbers, but we need constant space 
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?
- XOR
    - By using the XOR principle of Exclusive Or, any duplicates will result in zero. This leaves us with the single non-duplicate number.  

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create a hashset and count each number. Remove numbers seen a second time. There should be a single number left in hashset.

```markdown
1) Create hashset
2) Count each item
3) Remove numbers seen a second time
4) Return number with a single count
```

**General Idea:** Use XOR(Exclusive or) and the same number will return zero. Apply to all numbers and we isolate the non-duplicate number


```markdown
1) Create total variable
2) XOR each number
3) Return remaining number
```

⚠️ **Common Mistakes**

* Remember to use hashset uses linear space

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # Create hashset
        hashset = set()
        
        # Count each item
        for num in nums:
            # Remove numbers seen a second time
            if num in hashset:
                hashset.remove(num)
            else:
                hashset.add(num)
        
        # Return number with a single count
        return list(hashset)[0]
```

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # Create total variable
        total = 0

        # XOR each number
        for num in nums:
            total ^= num
        
        # Return remaining number
        return total
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in array 


* **Time Complexity**: O(N) we need to view each item in the array
* **Space Complexity**: O(1) using the XOR operator we only needed space for the total variable. 
    * Do note the Hashset uses O(N) because the hashset may store up to O(N/2) numbers before removing them upon seeing the numbers a second time.