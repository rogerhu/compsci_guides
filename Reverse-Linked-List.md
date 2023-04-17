## Problem Highlights

* 🔗 **Leetcode Link:** [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Linked List, Two Pointer
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
    - Yes. The linked list can be empty.
- What are the time and space constraints for this problem?
    - We are looking for a O(`N`) time and O(`1) space solution. `N` being the number of items in the array.

```markdown
HAPPY CASE
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]

Input: head = [5,4,3,2,1], left = 1, right = 5
Output: [1,2,3,4,5]

EDGE CASE
Input: head = []
Output: []
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - This may help us determine the length of the list. However we do not need the length of the list.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - There are no edge cases that a dummy head would help handle
- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. How could we use two different pointers in this problem? A previous point and a current pointer?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will initialize a previous node,current node, and a next node. We will then point the current node .next to the previous node and move the previous node to current node and current node to next node and repeat the process. 

```markdown
1) Initialize a previous node, a current node.
2) While current node is not null
    a) Initialize a next node to temporarily hold the next node
    b) Point current node .next to previous node
    c) Set prev node to current node
    d) Set current node to next node
3) Return previous node

```

**⚠️ Common Mistakes**

* Mutiple passes will slow down code and make code more complex.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Initialize a previous node, a current node.
        prev, curr = None, head

        # While current node is not null
        while curr:
            # Initialize a next node to temporarily hold the next node
            next = curr.next
            # Point current node .next to previous node
            curr.next = prev
            # Set prev node to current node
            prev = curr
            # Set current node to next node
            curr = next
        
        # Return previous node
        return prev
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list.

* **Time Complexity**: `O(N)` because we may need to traverse all the nodes in the linked list.
* **Space Complexity**: `O(1)` because we only need three pointers for memory.