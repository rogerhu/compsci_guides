## Problem Highlights

* 🔗 **Geeksforgeeks Link:**  [Reverse a doubly linked list in groups of given size](https://www.geeksforgeeks.org/reverse-doubly-linked-list-groups-given-size/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 40 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/),[Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is there anything we need to check before changing the head?
    - Before changing head, check for the cases like empty list and list with only one node.
- What is the structure of a doubly linked list?
    - A double linked list contains two pointers: a pointer to store the address of the next node (same as in singly linked list) and a pointer to store the address of the previous node.
- How is list reversal different in a singly linked list and a doubly linked list?
    - In a doubly linkst list, we have to reverse 2 links for each nodes.
- How do we swap nodes in a doubly linked list?
    - In a doubly linked list, each node stores reference to both next and previous nodes. For reversing a doubly linked list, for each node previous and next references should be swapped.
```markdown
HAPPY CASE
Input: 1 <-> 2 <-> 3 <-> 4 <-> 5 <-> 6 <-> 7 <-> 8, k = 3
Output: 3 <-> 2 <-> 1 <-> 6 <-> 5 <-> 4 <-> 8 <-> 7
Input: 1 <-> 2 <-> 3 <-> 4 <-> 5 <-> 6 <-> 7 <-> 8, k = 2
Output: 2 <-> 1 <-> 4 <-> 3 <-> 6 <-> 5 <-> 8 <-> 7
 
EDGE CASE
Input: 1 <-> 2 <-> 3 <-> 4 <-> 5 <-> 6 <-> 7 <-> 8, k = 10
Output: 8 <-> 7 <-> 6 <-> 5 <-> 4 <-> 3 <-> 2 <-> 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists, some things we should consider are:

- Multiple passes over the linked list 
    - Do we need to discover the length of the lists? No
- Dummy Head
    - Are we restructuring the given lists? Creating a new one? Yes; this is useful because we need to point a new head.

- Two Pointers
    - Two pointers would be useful in helping us find the node that we need to separate into pods


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** "chunk" the original linked list into "chunks" of size k. Then, reverse only that "chunk" of linked list. For instance, given [1, 2, 3, 4, 5, 6, 7] and k=3, handle the "chunks" [1, 2, 3] and [4, 5, 6] and [7]



```markdown
1. Create dummy node to reference new head 
2. Create previous pod to point at next reversed pod
3. Create current pod to be reveresed
4. While there is a pod to be reversed repeat the following 
    1. Create pointer to the next pod
    2. If there is a next pod disconnect it from the current pod
    3. Reverse the current pod and get the head and tail
    4. Set the previous pod to point to the new head
    5. Set the new head to point to the previous pod
    6. Set the new previous pod to the tail 
    7. Set the new current pod to next pod 
5.Set the head to dummy.next and disconnect head from dummy
6.Return the new head node
```

**⚠️ Common Mistakes**

* Look out to pop and push element to the right Stack
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def reverseListByPod(head: LinkedListNode, k: int) -> LinkedListNode:
    if k == -1:
        return None
    # Create dummy node to reference new head 
    dummyNode = LinkedListNode("H")
    
    # Create previous pod to point at next reversed pod
    prevPod = dummyNode
    
    # Create current pod to be reveresed
    currPod = head
    
    # While there is a pod to be reversed repeat the following 
    while currPod:
        # Get the next pod
        nextPod = getNextPod(currPod, k)
        
        # If there is a next pod disconnect it from the current pod
        if nextPod:
            nextPod.prev.next = None
            nextPod.prev = None
        
        # Reverse the current pod and get the head and tail
        head, tail = reverseList(currPod)
        
        # Set the previous pod to point to the new head
        prevPod.next = head
        
        # Set the new head to point to the previous pod
        head.prev = prevPod
        
        # Set the new previous pod to the tail 
        prevPod = tail
        
        # Set the new current pod to next pod 
        currPod = nextPod

    # Set the head to dummy.next and disconnect head from dummy
    head = dummyNode.next
    dummyNode.next = None
    head.prev = None
    
    # Return the new head node
    return head
        
def getNextPod(node: LinkedListNode, k: int) -> LinkedListNode:
    # Get the node k spaces away
    if k <= 0:
        return None
    
    while k and node:
        node = node.next
        k -= 1
    
    return node

def reverseList(head: LinkedListNode) -> (LinkedListNode, LinkedListNode):
    # Reverse Linked List and return the head and tail node
    if not head:
        return None
    dummyNode = LinkedListNode("H")
    dummyNode.next = head
    prev, curr = dummyNode, head
    
    while curr:
        next = curr.next
        curr.next = prev
        prev.prev = curr
        prev = curr
        curr = next
    head = prev

    # get tail node
    tail = dummyNode.prev
    # remove link from head node 
    head.prev = None
    # remove link to and from dummyNode
    dummyNode.prev.next = None
    dummyNode.next = None
    return (head, tail)

class LinkedListNode:
    def __init__(self, val, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev

    def __repr__(self):
        node = self
        vals =[]
        reverseVals=[]

        while node:
            vals.append(str(node.val))
            prev = node
            node = node.next
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the doubly linked list.

* **Time Complexity**: `O(N)` because we will need to access each node in the linked list.
* **Space Complexity**: `O(1)` because we only need the previous, current, and next pointers to solve this problem.