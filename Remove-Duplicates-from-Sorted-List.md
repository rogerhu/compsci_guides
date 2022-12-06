## Problem Highlights

* 🔗 **Leetcode Link:** [Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Linked List, Recursion
* 🗒️ **Similar Questions**: [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/), [Remove Duplicates From an Unsorted Linked List](https://leetcode.com/problems/remove-duplicates-from-an-unsorted-linked-list/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will the node values be given in sorted order?
    - Yes the list is guaranteed to be given sorted in ascending order
- Can the input list be empty?
    - Yes. The input list can have 0 to 300 nodes
- Is it guaranteed that the list will have duplicates?
    - No. The list can have unique values

   
```markdown
HAPPY CASE
Input: 0->1->1->2
Output: 0->1->2

Input: 1->3->4->4
Output: 1->3->4

EDGE CASE
Input: 1->1->2->3
Output: 1->2->3
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - Taking multiple passes of the list could tell us how many duplicates there are for each number. This could help us develop a solution.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - We are not rearranging the list. Rather, we are removing duplicate instances. A dummy head would help us if we were to remove the first element of the list. However, if the first element is duplicated we could remove the duplicates besides the first element, right?
- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. How could we use two different pointers in this problem? Would we have the pointers on two different ends of this list? Could the pointers reference the same node initially?


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Store all unique elements in a separate array, sorted. Create a new LinkedList from the unique elements.

```markdown
1) Create an array of size N
2) Iterate through the LinkedList
    a) If the element is not the same as the last added element to the 
        array, append it
3) Create a dummy head node to compare the current node with next nodes' data
4) Iterate through the array
    a) Re-assign “next” references of the LinkedList node to the value of the next index in the array
    b) The last element's 'next' in the LinkedList should be set to null
```

**General Idea:** Use two pointers to set the first instance of a duplicate's 'next' to next unique element.

```markdown
1) Establish two pointers
2) While the two pointers are not null
    a) Re-assign 'next' of each node to a node with a different value
    b) Move both pointers to the node with a different value
    c) Repeat
3. Return head node
```

**⚠️ Common Mistakes**

* The first approach takes N space and generates a new list, which may not be allowed depending on interviewer. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Establish two pointers
        prev = curr = head
        
        # While two pointers are not null
        while curr:
            # Re-assign 'next' of each node to a node with a different value
            while curr and prev.val == curr.val:
                curr = curr.next
            prev.next = curr
            
            # Move both pointers to the node with a different value
            prev = curr
        
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

* **Time Complexity**: `O(N)` because we need to traverse all nodes in the linked list.
* **Space Complexity**: `O(1)` because we only need two pointers for memory.