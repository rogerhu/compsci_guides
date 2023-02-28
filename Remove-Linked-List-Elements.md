## Problem Highlights

* 🔗 **Leetcode Link:** [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Linked List, Dummy Node, Recursion
* 🗒️ **Similar Questions**: [Remove Element](https://leetcode.com/problems/remove-element/), [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?


- Can the input list be empty?
    - Yes. The linked list can have 0-10000 nodes.

   
```markdown
HAPPY CASE
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]

Input: head = [7,7,7,7], val = 7
Output: []

EDGE CASE
Input: head = [], val = 1
Output: []
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - We are not trying to determine the length of the list, nor are we trying to determine something about the values stored at each node in the list. Taking multiple passes of the linked list will not help us.

- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - We are rearranging the list. We want a dummy node to account for the first node or all nodes being deleted.

- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers does not help us here.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Set previous node at dummy node. Take a pass through the linked list and keep and point previous node to node that does not equal value. 


```markdown
1) Initialize dummy node, previous node, current node
2) Iterate through LL (while the current node is not null)
    a) while current node is val, set current node to next
    b) if current node is not equal then set prev.next to curr.
    c) if curr exist, move prev to current node and current node to next and repeat
3) Return dummy.next
```

**General Idea:** Use recursion to point each node at the following node if they are equal to value.

```markdown
1) Set basecase where head is None
2) Set head.next to the recursive function of the next node to ensure the next node does not equal value
3) Establish logic for each head node, such that if it's value is equal to value, set this head node to be the next node
4) Return the head node
```

**⚠️ Common Mistakes**

* Remember to use a dummy node to handle edge cases.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Dummy Node**

```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # Initialize dummy node, previous node, current node
        prev = dummy = ListNode(val=0, next=head)
        curr = head

        # Iterate through LL (while the current node is not null)
        while curr:
            # while current node is val, set current node to next
            while curr and curr.val == val:
                curr = curr.next
            # if current node is not equal then set prev.next to curr.
            prev.next = curr

            # if curr exist, move prev to current node and current node to next and repeat
            if curr:
                prev = prev.next
                curr = curr.next
        # Return dummy.next
        return dummy.next
```
    
**Recursion**
```python    
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        # Set basecase where head is None
        if head == None:
            return None

        # Set head.next to the recursive function of the next node
        head.next = self.removeElements(head.next, val)

        # Establish logic for each head node, such that if it's value is equal to value, set this head node to be the next node
        if head.val == val:
            head = head.next
        
        # Return the head node
        return head
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list.

**Dummy Node**

* **Time Complexity**: `O(N)` because we need to traverse all nodes in the linked list to check for node with the value in question.
* **Space Complexity**: `O(1)` because we only need a couple pointers for memory.

**Recursion**
* **Time Complexity**: `O(N)` because we need to recursively traverse all nodes in the linked list to reach the basecase None.
* **Space Complexity**: `O(1)` because we only need a couple pointers for memory, but if we count the recursive stack it will be `O(N)`