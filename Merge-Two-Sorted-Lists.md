## Problem Highlights

* 🔗 **Leetcode Link:** [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Linked List, Dummy Node
* 🗒️ **Similar Questions**: [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/), [Sort List](https://leetcode.com/problems/sort-list/), [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we create new nodes?
  - No. The question asks you to splice the given nodes.
- Will the input lists be sorted?
  - Yes. This is stated in the problem statement.
- Can we expect empty input lists?
  - Yes. The lists may have 0-50 nodes
- Can the node values be negative?
  - Yes. The node values will fall between -100 and 100.
- Can the input lists have differing lengths?
  - Yes. Do not assume they will have the same length.
   
```markdown
HAPPY CASE
Input: 1->2->4, 1->3->4  
Output: 1->1->2->3->4->4

Input: 4->7->10 -4->3->4->5
Output: -4->3->4->4->5->7->10

EDGE CASE
Input: [ ], [ ] 
Output: [ ]

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists problems, we want to consider the following approaches:

- Multiple passes over the linked list. If we were able to take multiple passes of the linked list, would that help solve the problem?
  - We are not trying to determine the length of the list, nor are we trying to determine something about the values stored at each node in the list. We also only need to take one pass over each list to determine the values to be stored in the nodes of the return list.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
  - A dummy head can help us rearrange our list. Can you think of a way to use the dummy head to rearrange and incorporate values into one list from the other?
- Two Pointers. If we used two pointers to iterate through list, would that help us solve this problem?
  - Two pointers could help us in this problem. At what speed should we move the pointers? Where should both pointers point to initially?

**⚠️ Common Mistakes**

- Which of the above would provide us with an optimal solution?


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Iterate through both lists and store their values in an array. Sort the array. Rebuild the linked list with the values in the array.


```markdown
1) Initialize an empty array that will store our lists' values
2) Iterate through both LLs and add their values to the empty array
3) Sort the array
4) Iterate through the array, building a new LL with each node having a value from the array
5) Return the head of the new LL
```

**General Idea:** Build a new linked list. In each iteration, compare the head of the two linked lists. Whichever head has the smaller value gets added to the new linked list we're building. Continue iterating until one list is null, and then add the rest of the other list to our newly built list.


```markdown
1) Iterate over the LLs (while head1 or head2) where head1 and head2 are pointers to the heads of the input LLs
    a) If head1.val <= head2.val, create and append a node with a value of head1.val to the return list.
       Move head1 forward one node.
    b) Otherwise, create and append a node with value of head2.val to the return list
       Move head2 forward one node.
2) Figure out if either list still hasn't been fully traversed. 
   If it hasn't, finish traversing and adding its values to the return list
3) Return head of new LL
```

**General Idea:** Similar to the previous approach, but instead of building a new linked list we change the next pointers based on which value is smaller. We start with a dummy head. The dummy head's next will be whichever is the smaller head of the two linked lists. Iterate through this step until one of the list's head next is null. At which point we point it to the rest of the other list.


```markdown
1) Instantiate a new node with a value of 0. 
   This will be our dummy node, the node to the left of the head of our return list. 
   Store a reference to this node so that we can return the list at the end.
2) Create a pointer, tail, that always points to the end of our LL. Initialize it to point to the dummy node.
3) Iterate over the LLs (while head1 or head2) where head1 and head2 are pointers to the heads of the input LLs
    a) If head1.val <= head2.val, append head1 to the list by pointing tail's next pointer to head1.
       Move head1 forward one node.
    b) Otherwise, append head2 to the list by pointing tail's next pointer to head2.
       Move head2 forward one node
4) Move tail forward one node 
5) Figure out if either list still hasn't been fully traversed.
   If it hasn't, point tail's next to the head of the list that hasn't been fully traversed.
6) Return dummy.next
```



## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        # Instantiate a new node with a value of 0 + Create a pointer, tail, that always points to the end of our LL. Initialize it to point to the dummy node.
        dummyNode = tail = ListNode()
        
        # Iterate over the LLs (while head1 or head2) where head1 and head2 are pointers to the heads of the input LLs
        while list1 and list2:
            # If head1.val <= head2.val, append head1 to the list by pointing tail's next pointer to head1.
            if list1.val < list2.val:
                tail.next = list1
                list1 = list1.next
            else:
                # Otherwise, append head2 to the list by pointing tail's next pointer to head2
                tail.next = list2
                list2 = list2.next
                
            # Move tail forward one node 
            tail = tail.next
                
        # Figure out if either list still hasn't been fully traversed. +  If it hasn't, point tail's next to the head of the list that hasn't been fully traversed.
        if list1:
            tail.next = list1
        else:
            tail.next = list2
        
        # Return dummy.next
        return dummyNode.next
  
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list 1. `M` represents the number of nodes in the linked list 2. 

* **Time Complexity**: `O(N + M)` because we may need to traverse all numbers in both linked list
* **Space Complexity**: `O(1)` because only need to store the dummyNode as our head pointer and tail as our tail pointer.  