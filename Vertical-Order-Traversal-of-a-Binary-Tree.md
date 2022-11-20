## Problem Highlights

* 🔗 **Leetcode Link:** <>
* **Difficulty:** 
* **Time to complete**: __ mins
* **Topics**: Binary Trees
* **Similar Questions**: []()
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

-
   
```markdown
HAPPY CASE


EDGE CASE

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For trees, some things we should consider are:
- Using a traversal
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
- Using a binary search
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown

```

**⚠️ Common Mistakes**

* 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java

```
```python
 
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity:
* Space Complexity: