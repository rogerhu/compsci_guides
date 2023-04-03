## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Arithmetic Triplets](https://leetcode.com/problems/number-of-arithmetic-triplets/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Hashtable
* 🗒️ **Similar Questions**: [Two Sum](https://leetcode.com/problems/two-sum/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- 
   
```markdown

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:
* Sort. Sorting does not help solve this specific problem, because the ordering of the letters is important.
* Two pointer solutions (left and right pointer variables).Two pointer approach does not help solve this specific problem, because ordering of the letters is unidirectional.
* Storing the elements of the array in a HashMap or a Set. This could potentially work if know exactly what to look for after we store elements. Create dictionary where we are going to store the value and the index of each list element as a key-pair respectively. Then we iterate through the indices and values of the list containing our numbers. If the difference between the target and the current value in the list is already included as a key in the dictionary, then it means that the current value and the value stored in the dictionary is the solution to our problem.
* Traversing the array with a sliding window. A solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.




## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown

```

**⚠️ Common Mistakes**

* 

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



* **Time Complexity**: 

* **Space Complexity**: 