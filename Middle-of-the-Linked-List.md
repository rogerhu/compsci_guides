## Problem Highlights

* 🔗 **Leetcode Link:** [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Linked List, Two Pointer
* 🗒️ **Similar Questions**: [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/), [Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input list be empty?
    - No. The linked list can have 1-100 nodes.
- What is the middle of one node?
    -  Take the first node.
- What is the middle of two nodes?
    - Take the second node.
- What is the middle of three nodes?
    - Take the middle node
- What is the middle of four nodes? 
   - Between the two middle nodes, take the second one.
   ```markdown
    Input: 1 -> 2 -> 3 -> 4
    Output: 3 -> 4
    ```

```markdown
HAPPY CASE
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.

Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.

EDGE CASE
Input: head = [1]
Output: [1]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked List problems, we want to consider the following approaches:

- Multiple Pass. If we were able to take multiple passes of the linked list, would that help solve the problem?
    - This may help us determine the length of the list, and help us get the middle node. However this is not the most efficent approach as this would require two pass.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
    - We are not rearranging the list. Rather, we are trying to determine the structure of the linked list.
- Two Pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
    - Two pointers could help us in this problem. How could we use two different pointers in this problem? Would we have the pointers move at different speeds? Could the pointers reference the same node initially?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Every time we move slow pointer once, we move fast pointer twice. Once fast pointer reaches the end, the slow pointer is pointing at the middle node. 

```markdown
1) Initialize slow and fast pointer at head node
2) While fast pointer and fast.next pointer exist
    a) Move slow pointer forward once
    b) Move fast pointer forward twice 
3) Return slow pointer
```

**⚠️ Common Mistakes**

* Mutiple passes will slow down code and make code more complex.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Initialize slow and fast pointer at head node
        slow = fast = head
        # While fast pointer and fast.next pointer exist
        while fast and fast.next:
            # Move slow pointer forward once
            slow = slow.next
            # Move fast pointer forward twice
            fast = fast.next.next
        # Return slow pointer
        return slow
```
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        // Initialize slow and fast pointer at head node
        ListNode slow = head, fast = head;
        // While fast pointer and fast.next pointer exist
        while (fast != null && fast.next != null) {
            // Move slow pointer forward once
            slow = slow.next;
            // Move fast pointer forward twice
            fast = fast.next.next;
        }
        // Return slow pointer
        return slow;
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

* **Time Complexity**: `O(N)` because we need to traverse half the nodes in the linked list.
* **Space Complexity**: `O(1)` because we only need two pointers for memory.