## Problem Highlights

* 🔗 **Leetcode Link:** [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Linked List, Two Pointer, Recursion
* 🗒️ **Similar Questions**: [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/), [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
    - Yes. The linked list can be empty.

```markdown
HAPPY CASE
Input: head = [1,2,2,1]
Output: true

Input: head = [1,2]
Output: false

EDGE CASE
Input: head = []
Output: true
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - This may help us determine the length of the list. We can split linked list in half and reverse the second half. And check head and reversed nodes. 

- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - There are no edge cases that a dummy head would help handle.

- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. It helps with middle of linked list and reversing it.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** (Approach 1: Recursion Advanced) Use the recusive stack to check last node against the first node and repeat. 


```markdown
1) Initialize a front pointer
2) Define Recursive Call: To check front pointer against entire stack of nodes(reversed nodes).
    a) Recursive check on next node otherwise return false
    b) Ensure front pointer is equal to stack node otherwise return false
    c) Move front pointer.  
    At end of call stack return True
4) Return Recursive Call
```

**General Idea:** (Approach 2: Split/Reverse Linked List) Split the list in half and reverse half of it. Check the regular linked list and reverse linked list. 


```markdown!
1) Split list in half
2) Reverse second half of list
3) Check each node between regular linked list and reversed linked list
4) Return True if no differences were found
```

**⚠️ Common Mistakes**

* Mutiple passes will slow down code and make code more complex.

## 4: I-mplement

**Implement** the code to solve the algorithm.

Approach #1: Recursion Advanced
```python
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        # Initialize a front pointer
        self.frontPointer = head

        # Define Recursive Call: To check front pointer against entire stack of nodes(reversed nodes).
        def recursiveCheck(curr: Optional[ListNode]) -> bool:
            if curr:
                # Recursive check on next node otherwise return false
                if not recursiveCheck(curr.next):
                    return False
                # Ensure front pointer is equal to stack node otherwise return false
                elif self.frontPointer.val != curr.val:
                    return False
                # Move front pointer
                else:
                    self.frontPointer = self.frontPointer.next
            # At end of call stack return True
            return True
            
        # Return Recursive Call
        return recursiveCheck(head)

```

Approach #2: Split/Reverse Linked List
```python
class Solution:
    def isPalindrome(self, head: ListNode) -> bool:
        if not head or not head.next:
            return True

        # Split list in half
        
        # See [Middle of the Linked List](https://github.com/codepath/compsci_guides/wiki/Middle-of-the-Linked-List)
        prev, slow, fast = None, head, head
        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next
        prev.next = None
        l1, l2 = head, slow

        # Reverse second half of list
        rev = self.getReversed(l2)

        # Check each node between regular linked list and reversed linked list
        while l1 and rev:
            if l1.val != rev.val:
                return False
            l1, rev = l1.next, rev.next
        
        # Return True if no differences were found
        return True
    
    # See [Reversed Linked List](https://github.com/codepath/compsci_guides/wiki/Reverse-Linked-List)
    def getReversed(self, h):
        if not h:
            return h
        curr, prev = h, None
        while curr:
            nextNode = curr.next
            curr.next = prev
            prev = curr
            curr = nextNode
        return prev
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list.

* **Time Complexity**: `O(N)` because we need to traverse all the nodes in the linked list.
* **Space Complexity**: `O(1)` because we only need several pointers for memory in the split/reverse linked list solution.
    * Do note `O(N)` is required for the advanced recursion solution because we need to store the current node in the call stack.