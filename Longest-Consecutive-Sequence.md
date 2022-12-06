## Problem Highlights

* 🔗 **Leetcode Link:** [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array,  Hash, Sliding Window 
* 🗒️ **Similar Questions**: [Find Three Consecutive Integers That Sum to a Given Number](https://leetcode.com/problems/find-three-consecutive-integers-that-sum-to-a-given-number/),  [Length of the Longest Alphabetical Continuous Substring](https://leetcode.com/problems/length-of-the-longest-alphabetical-continuous-substring/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are there run time constraints?
    - O(N)
- Can extra memory be used?
    - O(N)
- What should we return if there is no subsequence with consecutive elements?
    - Return 0
- What should we return if there are duplicate elements in the subsequence?
    - Duplicates don't add to the count.
- What should we return for an empty array? 
    - Return 0
   
```markdown
HAPPY CASE
Input: nums = [100,4,200,1,3,2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

Input: nums = [0,3,7,2,5,8,4,6,0,1]
Output: 9

EDGE CASE
Input: [] 
Output: 0
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sorting the array and removing duplicates could help us determine the longest subarray with consecutive elements.
- Two pointer solutions (left and right pointer variables), while a two pointer method may help with our sorted array, this is not the most efficient method for this problem.
- Storing the elements of the array in a HashMap or a Set, by inserting all elements into a Set we can instantly remove duplicates from our list.
- Traversing the array with a sliding window, a solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Approach #1 Sort and find subsequence length
```markdown
1. Sort the array
2. Keep track of current length count (current) and longest length count (longest). Both starting at 1.
3. Loop through the sorted array 
4. Skip duplicates
5. If current number is one more than previous number, increment current streak 
6. Else set longest streak and reset current streak
7. Return the longest streak
```

**General Idea:** Approach #2 Use a hash set to find the longest length
```markdown
1. Keep track of longestStreak starting at 0.
2. Create a hashset 
3. Loop through the number set
4. If current number has no previous number, then start counting from this number
5. While the set contains currentNum + 1, increment currentNum and currentStreak
6. After each while loop, check and set longestStreak
7. Return longestStreak
```
**⚠️ Common Mistakes**

* If there is a runtime constraint of O(n), the second solution. If there is a memory constraint O(1), the first solution.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# Approach #1 Sort and find subsequence length

class Solution:
    def longestConsecutive(self, nums):
        if not nums:
            return 0
        # Sort the array
        nums.sort()
        
        # Keep track of current length count (current) and longest length count (longest). Both starting at 1.
        longest_streak = 1
        current_streak = 1

        # Loop through the sorted array 
        for i in range(1, len(nums)):
            # skip duplicates
            if nums[i] == nums[i-1]:
                continue
            
            # If current number is one more than previous number add to current streak 
            if nums[i] == nums[i-1]+1:
                current_streak += 1
            # Else set longest streak and reset current streak
            else:
                longest_streak = max(longest_streak, current_streak)
                current_streak = 1
        
        # Return the longest streak
        return max(longest_streak, current_streak)
```
```python
# Approach #2 Use a hash set to find the longest length

class Solution:
    def longestConsecutive(self, nums):
        # Keep track of longestStreak starting at 0.
        longest_streak = 0
        # Create a hashset 
        num_set = set(nums)
        
        # Loop through the number set
        for num in num_set:
            # If current number has no previous number, then start counting from this number
            if num - 1 not in num_set:
                current_num = num
                current_streak = 1
                # While the set contains currentNum + 1, increment currentNum and currentStreak
                while current_num + 1 in num_set:
                    current_num += 1
                    current_streak += 1
                
                # After each while loop, check and set longestStreak
                longest_streak = max(longest_streak, current_streak)
        
        # Return longestStreak
        return longest_streak
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(N)` because at the first glance, the while loop inside the for loop may seem like the time complexity is O(n * n). However, the while loop is only reached when the currentNum is the first of a subsequence, the while loop can only run for n iterations throughout the entire runtime of the function. So the runtime is actually O(n+n) = O(n).
* **Space Complexity**: `O(N)` because we store the numbers in a hashset. 