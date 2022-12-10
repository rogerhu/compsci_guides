## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/reverse-nodes-in-k-group/>
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: 15 to 17 mins
* 🛠️ **Topics**: Linked Lists
* 🗒️ **Similar Questions**: [Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/), [Reverse Nodes in Even Length Groups](https://leetcode.com/problems/reverse-nodes-in-even-length-groups/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Is a recursive solution acceptable here?
  - The problem statement clearly mentions that we are not to use any additional space for our solution. So no, a recursive solution is not acceptable here because of the space utilized by the recursion stack. 
- What if there are < k nodes left in the linked list?
  - The problem statement mentions that if there are < k nodes left in the linked list, then we don't have to reverse them

Run through a set of example cases:

```markdown
HAPPY CASE
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]

Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]

EDGE CASE
Input: head = [0], k = 1
Output: [0]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Multiple passes: To find the length, or save other information about the contents. If we were able to take multiple passes of the linked list, would that help solve the problem? Multiple passes would not help this problem since we aren’t collecting unique items or need any pre-processing.
- Two pointers: ‘Race car’ strategy with one regular pointer, and one fast pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
A slow and fast pointer may not help the problem.
- Dummy node: Helpful for preventing errors when returning ‘head’ if merging lists, deleting from lists. Would using a dummy head as a starting point help simplify our code and handle edge cases? Since we have to manipulate the order of the list, including the head of the list, a dummy head would help maintain a pointer to beginning of the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We still count `k` nodes at a time. If we find `k` nodes, then we reverse them.

```markdown
1) find size of list
2) find no. of times we need to reverse the list
3) start reversing it for that "times" for each group of size k
4) make sure to point start.next to the node containing remaining linked list elements
5) return the last node of the first group 
```

⚠️ **Common Mistakes**

* A base condition to pay attention to is: if k number of nodes are not left from node head, then return head itself as we don't have to reverse them. Write out the main recursion function, and in the base case advance k steps forwards, then if meet a null node return the current node, else goes into the recursion call which is made up simply by calling the main recursion function again with respect to the k-steps-forward node, then just call the reverseK function to reverse the next k nodes, and return the result of the reverseK function call.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def reverseKGroup(self, head: ListNode, k: int) -> ListNode:	
	# move min(k, len(list)) times
        cur = head
        count = 0                  
        while cur and count < k:        
            cur = cur.next
            count += 1
        
	# this check is needed in case remaining list is shorter than k
	# then we want to return head directly (no reversing)
        if count == k: 
	    # recursively get the first node of the next k nodes, call it `last`
	    # after we are done reversing current k, we point last of the current k node to `last`
            last = self.reverseKGroup(cur, k) 
            cur = head
            prev = None

	    # standard reverse linked list except condition is not while node but for count times
	    # https://www.geeksforgeeks.org/reverse-a-linked-list/ <- animation really helpful 
            for _ in range(count):
                next = cur.next
                cur.next = prev
                prev = cur
                cur = next        
            head.next = last
            return prev
        
        return head            
```
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        // find size of list
        int n = 0;
        ListNode current = head;
        while (current != null) {
            n++;
            current = current.next;
        }
        
        // find no. of times we need to reverse the list
        int times = n / k;
        
        // start reversing it for that "times" for each group of size k
        while (times > 0) {
            ListNode returnHead = null;
            ListNode start = head;
            ListNode startNext = null;
            ListNode end = null;

            ListNode prev = null;
            ListNode cur = head;
            ListNode ford = null;

            // case when we are reversing the first group of list
            // we need to save the last element of this group
            // because this will become the head in the returning statement
            if (times == n / k) {
                for (int i = 0; i < k; i++) {
                    ford = cur.next;
                    cur.next = prev;
                    prev = cur;
                    cur = ford;
                }

                returnHead = prev;
            }
            
            startNext = cur;
            
            // reverse this group by K elements by basic reversal algorithm
            for (int i = 0; i < k; i++) {
                ford = cur.next;
                cur.next = prev;
                prev = cur;
                cur = ford;
            }
            
            end = prev;
            start.next = end;
            start = startNext;
            times--;
        }
        
        // make sure to point start.next to the node containing remaining linked list elements
        start.next = cur;
        
        // return the last node of the first group 
        return returnHead;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N), since we process each node exactly twice. 
* **Space Complexity**: O(1)
