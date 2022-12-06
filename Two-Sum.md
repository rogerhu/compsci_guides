## Problem Highlights

* 🔗 **Leetcode Link:** [Two Sum](https://leetcode.com/problems/two-sum/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Hash Table
* 🗒️ **Similar Questions**: [3Sum](https://leetcode.com/problems/3sum/), [4Sum](https://leetcode.com/problems/4sum/), [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be no solution for the input parameter ?
  - You may assume that each input would have exactly one solution, and you may not use the same element twice.

- What is the time and space complexity?
    - Can you come up with an algorithm that is less than O(n2) time complexity?


```markdown
HAPPY CASE
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

Input: nums = [3,2,4], target = 6
Output: [1,2]

EDGE CASE (Multiple Spaces)
Input: nums = [3,3], target = 6
Output: [0,1]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?

- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us find the two sum pair that adds up to the target after sorting the array. However we will need a separate array to store the index.

- Storing the elements of the array in a HashMap or a Set
    - A hashset will be helpful here, because we can store the missing counterpart of the numbers we had seen. 

- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a hashmap to store the missing counterpart of the numbers we had seen and it's index. We can refer to the hashmap for the index once we find the counterpart.


```markdown
1) Create a hashmap
2) Iterate through the array
    1) If we see the counterpart in our hashmap then return the index of the counterpart and current index.
    2) Store the counterpart of the number we have seen and current index.
```

⚠️ **Common Mistakes**

* Remember to use a hashmap to store the index. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Create a hashmap
        hashmap = dict()

        # Iterate through the array
        for i, num in enumerate(nums):
            # If we see the counterpart in our hashmap then return the index of the counterpart and current index
            if num in hashmap:
                return [hashmap[num], i]

            # Store the counterpart of the number we have seen and current index
            hashmap[target-num] = i
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.


* **Time Complexity**: O(n), we may need to visit every item in the array to find the two sum pair.
* **Space Complexity**: O(n), we may need to  build a hashmap of each number and it's index.