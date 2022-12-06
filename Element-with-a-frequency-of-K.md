## Problem Highlights

* 🔗 **Leetcode Link:** N/A
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Hash, Sliding Window 
* 🗒️  **Similar Questions**: [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/), [Word Frequency](https://leetcode.com/problems/word-frequency/), [Most Frequent Even Element](https://leetcode.com/problems/most-frequent-even-element/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are there memory constraints?
  - Yes, O(n)
- What’s the required time complexity?
  - Yes, O(n)
- What should we return if there is more than one element of K frequency?
  - Return either number
- What should we return if there are no numbers of K frequency?
  - Return Null
   
```markdown
HAPPY CASE
Input: [1, 2, 3, 2, 1, 2, 3, 2, 1], k = 2 
Output: 3

Input: [1, 2, 3], k = 3
Output: null

EDGE CASE
Input: [1, 2, 2, 1, 4, 5], k = 2
Output: 1 or 2 

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sorting the array could help us group together the same elements making it easy to count the number of occurrences the element k appears.
- Two pointer solutions (left and right pointer variables) could work in determining when the element value changes, but would be dependent on a sorted array
- Storing the elements of the array in a HashMap or a Set, we could use a HashMap to keep track of a counter for each element k as we traverse the array for the first time.
- Traversing the array with a sliding window, a solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Approach #1 Sort the array and find the number 

```markdown
1. Sort the array
2. Keep a counter of number of times seen
3. Loop through the sorted array
4. If current number is equal to previous number increase n
5. else check 
    - if n equals k then return number
    - else reset seen to 0 and continue
6. If seen never equals k then return None
```

**General Idea:** Approach #2 Use a hash map to find number. 

```markdown
1. Create a hashmap
2. Loop through the array 
3. Count number times seen using hashmap
4. Loop through hashmap to check if seen is equal to k
5. Return number if seen is equal to k 
6. If seen never equals k then return None
```

**⚠️ Common Mistakes**

* If there is a runtime constraint of O(n) and space constraint of O(n), the second solution. If there is a runtime constraint of O(nlogn) and memory constraint of O(1), the first solution.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> int:
        # Sort the array
        nums.sort()
        
        # Keep a counter of number of times seen
        seen = 0
        
        # Loop through the sorted array
        for i in range(len(nums)):
            # If current number is equal to previous number increase n
            if nums[i] == nums[i - 1]:
                seen += 1
            else:
                # if seen equals k then return the number
                if seen == k:
                    return nums[i - 1]
                # else reset seen to 0 and continue
                else:
                    seen = 0

        # If seen never equals k then return None
        return None
```
```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> int:
        # Create a hashmap
        hashmap = dict()
        
        # Loop through the array
        for i in range(len(nums)):
            # Count number times seen using hashmap
            if nums[i] in hashmap:
                hashmap[nums[i]] += 1
            else:
                hashmap[nums[i]] = 1
        
        # Loop through hashmap to check if seen is equal to k
        for num, seen in hashmap.items:
            # Return number if found
            if seen == k:
                return num
        
        # If seen never equals k then return None
        return None
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in array.

* **Time Complexity**: `O(N)` because we loop through the array once.
* **Space Complexity**: `O(N)` because if each number in array is seen once, then each number in array is stored in hashmap.