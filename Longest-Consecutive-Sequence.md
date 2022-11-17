## Problem Highlights

* 🔗 **Leetcode Link:** [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)
* **Difficulty:** Medium
* **Time to complete**: __ mins
* **Topics**: Array, Hash 
* **Similar Questions**: [Find Three Consecutive Integers That Sum to a Given Number](https://leetcode.com/problems/find-three-consecutive-integers-that-sum-to-a-given-number/), [Length of the Longest Alphabetical Continuous Substring](https://leetcode.com/problems/length-of-the-longest-alphabetical-continuous-substring/)
    
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
* Sort the array
* Keep track of current length count (current) and longest length count (longest). Both starting at 0.
* Loop through the sorted array with index a in ascending order
	* If (array[a] == array[a-1]), continue // duplicates do not count
	* If (array[a] - array[a - 1]) == 1, currentStreak += 1
	* Else:
		* longestStreak = Math.max(longestStreak, currentStreak)
		* currentStreak = 1 //Reset
* Return Math.max(longestStreak, currentStreak)

Time Complexity: O(n log n)
Space Complexity: O(1)
```

**General Idea:** Approach #2 Use a hash set to find the longest length
```markdown
* Create a hash set s
* Loop through the array and add all the value to the set
* Keep track of longest length count (longest) starting at 0.
* Loop through the array again with index a
	* If set.contains(array[a] - 1), continue //this current number is not the start index of the subsequence
	* Else create two trackers variables (currentNum = array[a] and currentLength = 1)
		* While the set contains currentNum + 1, increment currentNum and currentLength
		* After the loop is done, update longest length count longest = Math.max(longest, currentLength)
* Return longestStreak

Time Complexity: O(n). At the first glance, the while loop inside the for loop may seem like the time complexity is O(n * n). However, the while loop is only reached when the currentNum is the first of a subsequence, the while loop can only run for n iterations throughout the entire runtime of the function. So the runtime is actually O(n+n) = O(n).
Space Complexity: O(n) 
```
**⚠️ Common Mistakes**

* If there is a runtime constraint of O(n), the second solution is better. If there is a memory constraint, the first solution is better.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python

```
```java

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of XXX.

* **Time Complexity**: `O(N)` because ...
* **Space Complexity**: `O(N)` because ...