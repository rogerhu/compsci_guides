## Problem Highlights

* 🔗 **Leetcode Link:** [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)
* **Difficulty:** Medium
* **Time to complete**: 25 mins
* **Topics**: Linked List, Dummy Node
* **Similar Questions**: [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/), [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements), [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
  - Yes. Assume the input list will have anywhere from 0 node to 300 nodes.
- Will the input list be sorted?
  - Yes. The input list will be sorted.

   
```markdown
HAPPY CASE
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]

Input: head = [1,2,2,2,5]
Output: [1,5]

EDGE CASE
Input: head = [1,1,1,2,3]
Output: [2,3]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists problems, we want to consider the following approaches:

- Multiple passes over the linked list. If we were able to take multiple passes of the linked list, would that help solve the problem?
  - Do we need to discover the length of the lists? 

- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
  - Are we restructuring the given lists? Creating a new one? This could be helpful in keeping track of our return list.

- Two Pointers. If we used two pointers to iterate through list, would that help us solve this problem?
  - Two pointers are used in the sense that we are traversing two separate lists. Multiple pointers for one list does not make sense here though because we are not trying to compare nodes in the list with other nodes in that same list.

**⚠️ Common Mistakes**

- Don't worry about using too many variables, use prev, curr, and next. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a previous node as a placeholder to demarcate where nodes have no longer duplicates and use it to point to the next node that has no duplicate. Repeat until end of linked list.


```markdown
1) Create a dummy head. This will be our reference to our return list.
2) Create a prev pointer to point to next node that has not duplicates
3) Create curr pointer to check if curr node has no duplicate
3) Traverse the lists while prev and curr is not None
    a) Check curr to see if it has duplicates
        i) If curr has duplicates, point prev.next at next unique node and bypass curr node. 
        ii) Else curr has no duplicates set prev pointer at curr
5) Return dummy.next
```
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Create a dummy head. This will be our reference to our return list.
        dummy = ListNode(val=0, next=head)
        # Create a prev pointer to point to next node that has not duplicates
        prev = dummy
        # Create curr pointer to check if curr node has no duplicate
        curr = prev.next
        
        # Traverse the lists while prev and curr is not None
        while prev and curr:
            
            # Check curr to see if it has duplicates
            next = curr.next
            
            # If curr has duplicates, point prev.next at next unique node and bypass curr node. 
            if next and curr.val == next.val:
                while next and curr.val == next.val:
                    next = next.next
                prev.next = next
                curr = next
                
            # Else curr has no duplicates set prev pointer at curr
            else:
                prev = curr
                curr = next
        
        # Return dummy.next
        return dummy.next
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list

* **Time Complexity**: `O(N)` because we need to traverse all numbers in linked list
* **Space Complexity**: `O(1)` because we only needed a dummy node, prev node, curr node, and next node. 