## Problem Highlights

* 🔗 **Leetcode Link:** N/A
* **Difficulty:** Medium
* **Time to complete**: __ mins
* **Topics**: Array, Hash 
* **Similar Questions**: 
    
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

* Sort
- Sorting the array could help us group together the same elements making it easy to count the number of occurrences the element k appears.
- Two pointer solutions (left and right pointer variables) could work in determining when the element value changes, but would be dependent on a sorted array
- Storing the elements of the array in a HashMap or a Set, we could use a HashMap to keep track of a counter for each element k as we traverse the array for the first time.
- Traversing the array with a sliding window, a solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Approach #1 Sort the array and find the number 

```markdown
* Sort the array
* Keep a counter n
* Loop through the sorted array with index a
  * If array[a] == array[a - 1], n += 1
  * Else:
    * if n == k, return array[a]
    * else, n = 0
* Return null

Time Complexity: O(n log n)
Space Complexity: O(1)
```

**General Idea:** Approach #2 Use a hash map to find number. 

```markdown
* Create a hash map h
* Loop through the array with index a
  * If h[array[a]] exists, h[array[a]] += 1
  * Else h[array[a]] = 1
* Loop throug hash map h with key i
  * if h[i] == k, return i
* Return null

Time Complexity: O(n)
Space Complexity: O(n)
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

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