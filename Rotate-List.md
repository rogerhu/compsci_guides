## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/rotate-list/](https://leetcode.com/problems/rotate-list/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Linked Lists, Two Pointer
* 🗒️ **Similar Questions**: [Rotate Array](https://leetcode.com/problems/rotate-array/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input be null?
  - Yes, a null check would be a great in this problem.
- Could the k-value be larger than the length of the list?
  - Yep! What happens if the `k` is larger than the length of the list? This discussion may lead the student to discover that `k % list_length` is important. Or it may happen later in the UMPIRE process.
- What direction do we rotate k-times?
  - Rightwards.
- Is k guaranteed to be non-negative?
  - We can assume that `k` is guaranteed to be non-negative.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: 1->2->3->NULL, k = 2
Output: 2->3->1->NULL

Input: 1->2->3->4->5->NULL, k = 3
Output: 3->4->5->1->2->NULL

EDGE CASE
Input: 1->2->3->4->5->NULL, k=10
Output: 1->2->3->4->5->NULL 
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists, the top three things we want to consider are:

- If we were able to take multiple passes of the linked list, would that help solve the problem?
   - Knowing the length of the list is important since we want to rotate only `k % len(list)` times. Note: This computation may not be necessary for some approaches.
- Would using a dummy head as a starting point help simplify our code and handle edge cases?
   - Since we are manipulating the order of the list, a dummy head may look like a good strategy, and it may work. However, we don’t necessarily need one to solve this problem.
- If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
   - A slow and fast pointer may not help the problem.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1) Calculate the size of the list
2) Make the list circular
3) Set the (LEN - K)th node to have a NULL next reference
4) Return the node after the (LEN - K)th node, since it is the new start
```

⚠️ **Common Mistakes**

* Some people get confused by a k-value that is larger than the length of the list
* Multiple moving parts of this problem cause students to get lost in the details

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head or k == 0:
            return head

        # Compute the length of the list
        list_len = 1
        counter = head
        while counter.next:
            list_len += 1
            counter = counter.next

        counter.next = head # Make list circular

        k = k % list_len # Calculate new rotate size
        new_end = head
        for _ in range(list_len - k - 1):
            new_end = new_end.next
        new_start = new_end.next
        new_end.next = None
        return new_start
```
```java
class Solution {
  public ListNode rotateRight(ListNode head, int k) {
    // base cases
    if (head == null) return null;
    if (head.next == null) return head;

    // close the linked list into the ring
    ListNode old_tail = head;
    int n;
    for(n = 1; old_tail.next != null; n++)
      old_tail = old_tail.next;
    old_tail.next = head;

    // find new tail : (n - k % n - 1)th node
    // and new head : (n - k % n)th node
    ListNode new_tail = head;
    for (int i = 0; i < n - k % n - 1; i++)
      new_tail = new_tail.next;
    ListNode new_head = new_tail.next;

    // break the ring
    new_tail.next = null;

    return new_head;
  }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N), where N is a number of elements in the list
* **Space Complexity**: O(1) since it's a constant space solution
