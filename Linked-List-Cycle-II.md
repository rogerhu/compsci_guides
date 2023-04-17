## Problem Highlights

* 🔗 **Leetcode Link:** [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Linked List, Two Pointer, Hash, Recursion
* 🗒️ **Similar Questions**: [Happy Number](https://leetcode.com/problems/happy-number/), [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)
    
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
Output: 1
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

Input: head = [1,2], pos = 0
Output: 0
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

EDGE CASE
Input: head = [1], pos = -1
Output: -1
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

**General Idea:** First detect whether or not a cycle exist. Second determine the location of the cycle. 

```markdown!
1. Detect Cycle
    a. Instantiate two pointers where slow points to the head of the list and fast points to head
    b. Move slow once and fast twice until they meet
    c. If fast or fast.next is None then we have reached the end of cycle and return None
2. Determine the distance between head node and slow.
    a. Lets say H: is distance from head to cycle entry E slow is currently H + D
    b. Lets say D: is the distance from E to X
    c. Lets say X: is the location slow meets fast
    d. Lets say L: cycle length

                          _____
                         /     \
        head_____H______E       \
                        \       /
                         X_____/   
        
    
    e. When slow catches fast, slow traveled H + D and fast traveled 2(H + D)
    f. Assume fast traveled n loops in cycle
        - 2H + 2D = H + D + L
        - H + D = nL
        - H = nl - D
    g. Therefore if two pointers start from head and the other from X, both reaches E at the same time, because slow and fast met at -D. 
3. Shift head node and E until they meet and return
```

**⚠️ Common Mistakes**

* Can you solve it in constant memory? 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Instantiate two pointers where slow and fast points to the head of the list 
        slow = fast = head
        # Move slow once and fast twice until they meet
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break
        # If fast or fast.next is None then we have reached the end of cycle and return None
        if not fast or not fast.next: 
            return None
        
        # Find the distance from H to point of entry to cycle using theory(see above)
        # Shift head node and E until they meet and return
        e = slow
        h = head
        while e.next:
            if e == h:
                return e
            e = e.next
            h = h.next
```
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // Instantiate two pointers where slow and fast points to the head of the list 
        ListNode slow = head, fast = head;

        // Move slow once and fast twice until they meet
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) break;
        }

        // If fast or fast.next is None then we have reached the end of cycle and return None
        if (fast == null || fast.next == null) return null;

        // Find the distance from H to point of entry to cycle using theory(see above)
        while (head != slow) {
            head = head.next;
            slow = slow.next;
        }
        return head;
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

* **Time Complexity**: `O(N)` because we may need to traverse all nodes in the linked list at least twice. 
* **Space Complexity**: `O(1)` because we only need a few pointers for memory.