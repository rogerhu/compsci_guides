## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/integer-replacement/](https://leetcode.com/problems/integer-replacement/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Recursion, Memoization
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- 

- 


```markdown
Example 1:

Input: n = 8
Output: 3
Explanation: 8 -> 4 -> 2 -> 1

Example 2:

Input: n = 7
Output: 4
Explanation: 7 -> 8 -> 4 -> 2 -> 1
or 7 -> 6 -> 3 -> 2 -> 1

Example 3:

Input: n = 4
Output: 2

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


The strategy is to avoid repeatedly solving sub-problems. That is a clue to use memoization. Memoization is a way to potentially make functions that use recursion run faster. A recursive function might end up performing the same calculation with the same input multiple times. Memoizing allows us to store input alongside the result of the calculation. Therefore, rather than having to do the same work again using the same input, it can simply return the value stored in the cache.

**⚠️ Common Mistakes**

* Without memoization, we cannot create a cache where we store inputs with their calculated results. Whenever we have an input that we’ve already seen, we can simply retrieve the result rather than redoing any of our work. By creating a cache, we’re using additional space, so you have to decide whether that’s worth the improved speed. If you have a very large range of inputs where it’s fairly unlikely that you’ll need to repeat the same calculations, memoization may not be an efficient solution.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 


```markdown

```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python

       
```

``java

       
```


    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: 

* **Space Complexity**: 