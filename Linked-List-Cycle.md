## Problem Highlights

* 🔗 **Leetcode Link:** [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
* **Difficulty:** Easy
* **Time to complete**: __ mins
* **Topics**: Linked List, Two Pointer, Hash, Recursion,
* **Similar Questions**: [Happy Number](https://leetcode.com/problems/happy-number/), [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can there be more than one cycle in the linked list?
    - No. Each node has only one next pointer. Therefore the cycle can only occur at the end of the linked list. Either a node points to a new node ahead of it, or it cycles back to a previous node.
- Can the input list be empty?
    - Yes. The linked list can have 0-10000 nodes.

   
```markdown
HAPPY CASE
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

EDGE CASE
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - We are not trying to determine the length of the list, nor are we trying to determine something about the values stored at each node in the list. Taking multiple passes of the linked list will not help us determine whether or not there is a cycle.

- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - We are not rearranging the list. Rather, we are trying to determine the structure of the linked list.

- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. How could we use two different pointers in this problem? Would we have the pointers move at different speeds? Could the pointers reference the same node initially?



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Take a pass through the linked list and keep a counter of how many nodes we've seen. If the counter exceeds 1000 (the max nodes allowed), then we know there was a cycle.


```markdown
1) Initialize a variable counter to 0
2) Iterate through LL (while the node is not Null)
    a) If the counter > 1000, return True
    b) If the node is not Null, increment the counter
3) Return False (the while loop condition has been broken, we have reached the end of the list)
```

**General Idea:** Use two pointer method: Have two references to the list and move them at different speeds. Move one forward by 1 node and the other by 2 nodes. If the linked list has a loop the two pointers will meet, otherwise one of the pointers will eventually become null (when it has reached the end).


```markdown
1. Instantiate two pointers where slow points to the head of the list and fast points to head.next
2. Iterate over the LL (while slow != fast)
3. If slow is ever equal to fast then the while loop condition is broken and cycle is found.
4. If fast or fast.next is None this generates an exception and return False
```

**⚠️ Common Mistakes**

* The first approach takes N space, if there are N nodes, which may not be allowed depending on interviewer. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        try:
            # 1. Instantiate two pointers where slow points to the head of the list and fast points to head.next
            slow = head
            fast = head.next
            
            # Iterate over the LL (while slow != fast)
            while slow != fast:
                slow = slow.next
                fast = fast.next.next
                
            # If slow is ever equal to fast then the while loop condition is broken and cycle is found.
            return True
        except:
            # If fast or fast.next is None this generates an exception and return False
            return False
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