## Problem Highlights

* 🔗 **Leetcode Link:** <>
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 10 to 12 mins
* 🛠️ **Topics**: Linked Lists
* 🗒️ **Similar Questions**: []()
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- 

Run through a set of example cases:

```markdown
HAPPY CASE


EDGE CASE

```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Multiple passes: To find the length, or save other information about the contents. If we were able to take multiple passes of the linked list, would that help solve the problem? Multiple passes would not help this problem since we aren’t collecting unique items or need any pre-processing.
- Two pointers: ‘Race car’ strategy with one regular pointer, and one fast pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
A slow and fast pointer may not help the problem.
- Dummy node: Helpful for preventing errors when returning ‘head’ if merging lists, deleting from lists. Would using a dummy head as a starting point help simplify our code and handle edge cases? Since we have to manipulate the order of the list, including the head of the list, a dummy head would help maintain a pointer to beginning of the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1)
```

⚠️ **Common Mistakes**

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
