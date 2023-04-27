## Problem Highlights

* 🔗 **Leetcode Link:** [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Linked List, Two Pointer, Dummy Node
* 🗒️ **Similar Questions**: [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/), [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
    - No. The linked list can have 1-500 nodes.
- Can the left node be greater than the right node?
    - No. The left node is alway lesser than or equal to right node.
- What should we expect for time and space complexity?
    - Assume `N` represents the number of nodes in the linked list. `O(N)` time and `O(1)` Space, ignoring the recursive stack.

```markdown
HAPPY CASE
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]

Input: head = [1,2,3,4,5], left = 1, right = 5
Output: [5,4,3,2,1]

EDGE CASE
Input: head = [5], left = 1, right = 1
Output: [5]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - This may help us determine the length of the list. This will help us because we need to know where to start reversing
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - We may need to swap the first node for the last node, this requires a dummy node. 
- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. How could we use two different pointers in this problem? A previous point and a current pointer?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will reverse everything in between the left and right index. Then we will point to the new head and tail. 

```markdown
1) Eliminate Edge Case, left and right is equal
2) Initialize dummy node, left previous pointer, left pointer
3) Move left previous pointer to left previous and left pointer to left
4) Reverse all nodes from left pointer until right pointer
5) Insert reversed section into linked list
```

**⚠️ Common Mistakes**

* Multiple passes will slow down code and make code more complex.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def reverseBetween(self, head: Optional[ListNode], left: int, right: int) -> Optional[ListNode]:
        # Eliminate Edge Case, left and right is equal
        if left == right:
            return head

        # Initialize dummy node, left previous pointer, left pointer
        leftPrevious = dummy = ListNode(0,head)
        leftPointer = head

        # Move left previous pointer to left previous and left pointer to left
        for i in range(left-1):
            leftPrevious = leftPointer
            leftPointer= leftPointer.next
        
        # Reverse all nodes from left pointer until right pointer
        prev = None
        curr = reversedEndPointer = leftPointer
        for i in range(right-left+1):
            next = curr.next
            curr.next = prev
            prev = curr
            curr = next

        # Insert reversed section into linked list
        reversedStartPointer = prev
        restartPointer = curr

        reversedEndPointer.next = restartPointer
        leftPrevious.next = reversedStartPointer
        return dummy.next
```
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // Initialize dummy node, left previous pointer, left pointer
        ListNode dummy = new ListNode(0); 
        dummy.next = head;
        ListNode prev = dummy; 
        
        // Move left previous pointer to left previous and left pointer to left
        for(int i = 0; i < left - 1; i++)
            prev = prev.next; 
        
        // Reverse all nodes from left pointer until right pointer
        ListNode curr = prev.next; 
        for(int i = 0; i < right - left; i++){
            ListNode forw = curr.next;
            curr.next = forw.next;
            forw.next = prev.next;
            prev.next = forw;
        }
        
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

Assume `N` represents the number of nodes in the linked list.

* **Time Complexity**: `O(N)` because we may need to traverse all the nodes in the linked list.
* **Space Complexity**: `O(1)` because we only need several pointers for memory.