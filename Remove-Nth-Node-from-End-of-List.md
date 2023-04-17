## Problem Highlights

* 🔗 **Leetcode Link:** [Remove Nth Node from End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Linked List, Dummy Node, Two Pointer
* 🗒️ **Similar Questions**: [Delete N Nodes After M Nodes of a Linked List](https://leetcode.com/problems/delete-n-nodes-after-m-nodes-of-a-linked-list/), [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/), [Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
  - Yes. Assume the input list will have anywhere from 0 node to 30 nodes.
- Will the value of n be valid?
  - Yes. Assume n will be between 1 and the number of nodes in the list.

   
```markdown
HAPPY CASE
Input: 1->2->3->4->5, n = 2
Output: 1->2->3->5

Input: 1->2, n = 1
Output: 1

EDGE CASE
Input: 1->2, n = 2
Output: 2

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists problems, we want to consider the following approaches:

- Multiple passes over the linked list. If we were able to take multiple passes of the linked list, would that help solve the problem?
  - Do we need to discover the length of the lists? This might be useful when trying to delete a node from the end of the list.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
  - Are we restructuring the given lists? Creating a new one? Yes; this could be useful when trying to delete the first node in the list.
- Two Pointers. If we used two pointers to iterate through list, would that help us solve this problem?
  - Two pointers would be useful in helping us find the node that we're trying to remove, and then allow us to restructure the pointers to "remove" the node from the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Find the nth node and skip it.

```markdown
1) Create a dummy node and attach it to the head of the input list.
2) Initialize 2 pointers, first and second, to point to the dummy node.
3) Advance the first pointer so that the gap between the first and second pointers is n nodes
4) While the first pointer does not equal null move both first and second to maintain the gap and get nth node from the end
5) Delete the node being pointed to by second.
6) Return dummy.next
```

**⚠️ Common Mistakes**

- Not recognizing the dummy head technique as useful. The dummy head allows us to easily remove the value at the head of the list.

- Thinking that you have to replace the nth node to delete it. Usually with linked list problems where we want to delete a node, we end up restructure pointers that point to and from that node to remove it from the list.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        # Create a dummy node and attach it to the head of the input list.
        dummy = ListNode(val=0, next = head)
        
        # Initialize 2 pointers, first and second, to point to the dummy node.
        first = dummy
        second = dummy
        
        # Advances first pointer so that the gap between first and second is n nodes apart
        for i in range(n+1):
            first = first.next
            
        # While the first pointer does not equal null move both first and second to maintain the gap and get nth node from the end
        while (first != None):
            first = first.next
            second = second.next
        
        # Delete the node being pointed to by second.
        second.next = second.next.next
        
        # Return dummy.next
        return dummy.next
```
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Create a dummy node and attach it to the head of the input list.
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        // Initialize 2 pointers, first and second, to point to the dummy node
        ListNode first = dummy;
        ListNode second = dummy;
        
        // Advance the first pointer so that the gap between the first and second pointers is n nodes
        for (int i = 1; i <= n + 1; i++) {
            first = first.next;
        }
        
        // While the first pointer does not equal null move both first and second to maintain the gap and get nth node from the end
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        
        // Delete the node being pointed to by second.
        second.next = second.next.next;
        
        //Return dummy.next
        return dummy.next;
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list

* **Time Complexity**: `O(N)` because we need to traverse all numbers in linked list
* **Space Complexity**: `O(1)` because we only needed a dummy node, first node, second node