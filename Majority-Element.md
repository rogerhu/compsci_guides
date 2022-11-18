## Problem Highlights

* 🔗 **Leetcode Link:** [Majority Element](https://leetcode.com/problems/majority-element/)
* **Difficulty:** Easy
* **Time to complete**: __ mins
* **Topics**: Array, Hash, Divide And Conquer, Sorting
* **Similar Questions**: [Check If a Number Is Majority Element in a Sorted Array](https://leetcode.com/problems/check-if-a-number-is-majority-element-in-a-sorted-array/), [Most Frequent Even Element](https://leetcode.com/problems/most-frequent-even-element/), [Majority Element II](https://leetcode.com/problems/majority-element-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Why is the n/2 element considered to be the middle one in an array of an odd length?
    - Since majority element always exists and for sure it occurrs more than half of the total size of the array, hence middle element (incase of odd) and (middle-1)th or (middle+1)th element (incase of even) will always be the majority element.
    - There might not be a solution present. In that case, let’s return Null.
- Can you sort the array?
    - After sorting the array, the majority element shows up in the middle point. This works for both odd and even lengths' arrays. The solution is based on the definition of the majority element: it should occur in the list more than [n / 2]. It means that if we have a sorted list, the element in the middle will always be the majority element. The list can have odd or even number of elements. Sorted list with odd number of elements: [1,1,2]. The middle element is the majority element. Sorted list with even number of elements: [1,1,1,3]. Both elements in the middle will be the majority element.
   
```markdown
HAPPY CASE
Input: nums = [3,2,3,3]
Output: 3

Input: nums = [2,2,1,1,1,2,2]
Output: 2

EDGE CASE
Input: nums = [3,2,3]
Output: 3
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Arrays problems, we want to consider the following approaches:

- Sort. We can sort the array first and then use a sliding window to find the number with a frequency of n/2 + 1. However this will cost O(nlogn) time.
- Two pointer solutions (left and right pointer variables). Two pointer approach could work in this situation but relies on a sorted array, which does not solve for the specific problem.
- Storing the elements of the array in a HashMap or a Set. This could potentially work if know exactly what to look for after we store elements. You can do this by creating a dictionary where we are going to store the value and the index of each list element as a key-pair respectively. Then we iterate through the indices and values of the list containing our numbers. If the difference between the target and the current value in the list is already included as a key in the dictionary, then it means that the current value and the value stored in the dictionary is the solution to our problem.
- Traversing the array with a sliding window. A solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases, requires sorting.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Count each number using a hash table, once we reach n/2 + 1 counts, then return that number
```markdown
1) Create a hash table
2) Loop over the array. Add number to the hash table 
3) If the number of counts reach n/2 + 1 return that number
```

**⚠️ Common Mistakes**

* We need to find the element that appears more than n / 2 times. Initially what we can do is store each element as key and their occurrences as value in a Hash Map. Then iterate over the Hash Map and look for the pair having value more than n/2. If found then return the key else return -1. Now this approach has a O(n) time complexity and O(n) space complexity. We can further improve its space complexity to O(1).

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        # Create a hash table
        hashtable = {}
        
        # Loop over the array. Add number to the hash table 
        for num in nums:
            if num in hashtable.keys():
                hashtable[num] += 1  
            else:
                hashtable[num] = 1 
            
            # If the number of counts reach n/2 + 1 return that number
            if hashtable[num] > (len(nums) / 2):
                return num
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of the numbers in the array.

* **Time Complexity**: `O(N)` because we may iterate through the entire array to find the number with more than n/2 counts.
* **Space Complexity**: `O(N)` because we may be storing n/2 different numbers in our hash table.