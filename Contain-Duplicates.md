## Problem Highlights

* 🔗 **Leetcode Link:** [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Sort, Hash
* 🗒️ **Similar Questions**: [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/), [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input array be empty?
    - No. There must be at least a single number in the array of numbers. 

   
```markdown
HAPPY CASE
Input: nums = [1,2,3,1]
Output: true
Explanation: There are two 1 found in this array of numbers.

Input: nums = [1,2,3,4]
Output: false
Explanation: There are no duplicates found in this array of numbers.

EDGE CASE
Input: head = [1]
Output: false
Explanation: There are no duplicates found in this array of numbers
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. We can sort the array of numbers and check if the next item is equal to the prev item. Once we find a match, we can return True. Otherwise we reach the end of the array and return False.
- Two pointer solutions (left and right pointer variables). We will need to sort the array first to use the two pointer solution, but it really isn't all that helpful when sort does the job.
- Storing the elements of the array in a HashMap or a Set. As we iterate through the array, we can store each number in a Set. If the number is already in the Set, then we can return True. Otherwise we reach the end of the array and return False.
- Traversing the array with a sliding window. Similar to the two pointer solution. We will need to sort the array first to use the sliding window, but it really isn't all that helpful when sort does the job.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create a Set to store number. If the number is already in the Set, then return True. Otherwise we reach the end of the array and return False.


```markdown
1) Create Set
2) Iterate through numbers
    a) If number is already in set return True
    b) Store number in set
3) Return False (we have reached the end of the list without duplicate)
```

**⚠️ Common Mistakes**

* Sorting requires a O(NlogN) runtime, while a Hash requires a O(N) runtime. If we want to speed up our algorithm we should use a hash.
* Hash requires a O(N) space, while a Sorting requires a O(1) runtime. If we want to save space we should use sorting.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        # Create Set
        hashSet = set()
        
        # Iterate through numbers
        for num in nums:
            # If number is already in set return True
            if num in hashSet:
                return True
                
            # Store number in set
            hashSet.add(num)
        
        # Return False (we have reached the end of the list without duplicate)
        return False
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(N)` because we need to traverse all numbers in the array.
* **Space Complexity**: `O(N)` because we may need to store all numbers in the array. 