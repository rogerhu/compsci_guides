## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/swap-nodes-in-pairs/](https://leetcode.com/problems/swap-nodes-in-pairs/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Linked Lists, Recursion
* 🗒️ **Similar Questions**: [Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Could the input node be null?
  - I think we should account for that. It’d be a standard edge case in an interview setting.
- Is the list guaranteed to have an even length?
  - The list can be any length, even just 1 node.
- The numerical value in the nodes does not matter, right?
  - The solution to this problem shouldn’t have to worry about specific node values.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: head = [1,2,3,4]
Output: [2,1,4,3]

Input: head = []
Output: []

EDGE CASE
Input: head = [1]
Output: [1]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Multiple passes: To find the length, or save other information about the contents. If we were able to take multiple passes of the linked list, would that help solve the problem? Multiple passes would not help this problem since we aren’t collecting unique items or need any pre-processing.
- Two pointers: ‘Race car’ strategy with one regular pointer, and one fast pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
A slow and fast pointer may not help the problem.
- Dummy node: Helpful for preventing errors when returning ‘head’ if merging lists, deleting from lists. Would using a dummy head as a starting point help simplify our code and handle edge cases? Since we have to manipulate the order of the list, including the head of the list, a dummy head would help maintain a pointer to beginning of the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1) Establish a dummy head that points to the first node
2) Keep a pointer to the dummy head
3) Iterate through the list while the pointer has TWO valid nodes ahead
4) Swap the two nodes ahead of the pointer
5) Move the pointer to the second post-swap node
6) Continue to iterate until end of list
```

⚠️ **Common Mistakes**

* The trick is updating the pointers carefully. Key takeaway is: to avoid mistakes first update the deepest pointers, e.g. .next.next, then more shallow ones, etc. Try using a simple recursive technique to swap adjacent pairs. We swap the 2 adjacent nodes and find the next of swapped second adjacent node through recursion. Base case occurs when either the node passed contains null value aur adjacent node of passed node does not exist.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        def swap_node(node1, node2):
            subsequent_nodes = node2.next
            node2.next = node1
            node1.next = subsequent_nodes
            return node2, node1
        
        # less than two nodes, return as it is
        if not head or not head.next:
            return head
        
        # take first two nodes as cur and cur_next
        cur, cur_next = head, head.next
        res_head = None
        prev = None
        while cur and cur_next:
            # swap cur and cur_next
            cur, cur_next = swap_node(cur, cur_next)
            if prev:
                # set next of prev
                prev.next = cur
            if res_head is None:
                # set result head if its unset
                res_head = cur
            # set new cur and cur_next
            new_cur, new_cur_next = cur_next.next, None
            if new_cur:
                new_cur_next = new_cur.next
            prev = cur_next
            cur, cur_next = new_cur, new_cur_next
        return res_head
```
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode d = new ListNode(-1);
        d.next = head;
        // initialize  one dummy node and mark it as prev node
        ListNode prev = d;
        while(head != null && head.next!=null){
            // get the current node and next node
            ListNode a = head;
            ListNode b = head.next;
	    // now previous node's next node becomes b
            prev.next = b;
	    //  connect b nodes next to a nodes next to preserve chaining 
            a.next = b.next;	
	    // to make a node comes after b,
            b.next = a;
            // now a node becomes previous 	
            prev = a;
	    // iterate to next pair of nodes 
            head= a.next;
        } 
        return d.next;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: Time Complexity: O(N), as we used a single pass-through linked list
* **Space Complexity**: Space Complexity: O(1), as only 1 dummy node created for any size list
