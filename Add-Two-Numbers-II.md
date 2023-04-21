## Problem Highlights

* 🔗 **Leetcode Link:** [Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Linked List, Multiple Passes, Recursion
* 🗒️ **Similar Questions**: [Add Binary](https://leetcode.com/problems/add-binary/), [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/), [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can either input list be empty?
  - No. Each list will have anywhere from 1 to 100 nodes
- Will the node values be valid
  - Yes. The node values will be between 0 and 9
- Can the list represent a number that has leading zeroes?
  - No. The lists will not represent a number with leading zeroes.
- Can I reverse the input list and solve like Add Two Numbers?
    - No. You may not reverse the input list.
- What is the runtime and space complexity?
    - The runtime is constant O(n) and space is O(1) not counting memory stack.

```markdown
HAPPY CASE
Input: l1 = [7,2,4,3], l2 = [5,6,4]
Output: [7,8,0,7]

Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [8,0,7]

EDGE CASE
Input: l1 = [0], l2 = [0]
Output: [0]

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Linked Lists problems, we want to consider the following approaches:

- Multiple passes over the linked list. If we were able to take multiple passes of the linked list, would that help solve the problem?
  - Do we need to discover the length of the lists? This might be useful.
- Dummy Head. Would using a dummy head as a starting point help simplify our code and handle edge cases?
  - Are we restructuring the given lists? Creating a new one? This could be helpful in keeping track of our return list.
- Two Pointers. If we used two pointers to iterate through list, would that help us solve this problem?
  - Two pointers are used in the sense that we are traversing two separate lists. Multiple pointers for one list does not make sense here though because we are not trying to compare nodes in the list with other nodes in that same list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Obtain the length of both linked list. Recursively iterate through both lists and sum the values of the nodes and remainder from the previous addition for each node of equal length. 

```markdown
1) Get the length of the linked lists
2) Add the linked lists recursively
    a) Basecase: End of List -> Return None and carry value of 0
    b) Get Current Sum and Current Carry Value to recursively pass back to calling function
        i) Get Current Sum  by adding value at l1, l2, and carry value.
            1. If len1 > len2, then recursively call l1.next, to get next head node and carry value, as this signifies adding l1 value and 0
            2. If len2 > len1, then recursively call l2.next, to get next head node and carry value, as this signifies adding l2 value and 0
            3. If len1 == len2, then recursively call l1.next and l2.next, to get next head node and carry value, as this signifies adding both values
        ii) Get Current Carry Value by floor divide Current Sum
        iii) Get Current Sum Digit by module Current Sum
    c) Return Current Node with Current Sum Digit as value and previous head node as next node AND Current Carry Value
3) After recusive call, if there is carry over value, set as head node
4) Return the head node
```

**⚠️ Common Mistakes**

- Which of the above would provide us with an optimal solution?
    - First of all, the only way to add the numbers without reversing the linked list is to progress through the linked list recurively to use the memory stack as our reverse.
    - We will also need to go with multiple passes. Once we know the length of the linked list, we can adding digits that are in the same position. For example 456 + 23, we can skip 4 and add 56 and 23. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # Get the length of the linked lists
        len1, len2 = self._getLength(l1), self._getLength(l2)

        # Add the linked lists recursively
        head, carry = self._addRecursively(l1, len1, l2, len2)

        # After recusive call, if there is carry over value, set as head node
        if carry:
            head = ListNode(carry, head)
        
        # Return the head node
        return head

    # Add the linked lists recursively
    def _addRecursively(self, l1: Optional[ListNode], len1: int, l2: Optional[ListNode], len2: int) -> (Optional[ListNode], int):
        # Basecase: Return None and carry value of 0
        if not l1 and not l2:
            return (None, 0)

        # Get Current Sum and Current Carry Value to recursively pass back to calling function
        current_sum = 0
        current_carry = 0

        # Get Current Sum by adding value at l1, l2, and carry value.
        if len1 > len2:
            # If len1 > len2, then recursively call l1.next, to get next head node and carry value, as this signifies adding l1 value and 0
            head, carry = self._addRecursively(l1.next, len1 - 1, l2, len2)
            current_sum = l1.val + 0 + carry
        elif len1 < len2:
            # If len2 > len1, then recursively call l2.next, to get next head node and carry value, as this signifies adding l2 value and 0
            head, carry = self._addRecursively(l1, len1, l2.next, len2 - 1)
            current_sum = 0 + l2.val + carry
        else:
            # If len1 == len2, then recursively call l1.next and l2.next, to get next head node and carry value, as this signifies adding both values
            head, carry = self._addRecursively(l1.next, len1 - 1, l2.next, len2 - 1)
            current_sum = l1.val + l2.val + carry
        
        # Get Current Carry Value by floor divide Current Sum
        current_carry = current_sum // 10

        # Get Current Sum Digit by module Current Sum
        current_sum_digit = current_sum % 10

        # Return Current Node with Current Sum Digit as value and previous head node as next node AND Current Carry Value
        return (ListNode(current_sum_digit, head), current_carry)

    def _getLength(self, head: ListNode) -> int:
        result = 0
        curr = head

        while curr:
            curr = curr.next
            result += 1
        
        return result
```
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // Get the length of the linked lists
        int len1 = getLength(l1);
        int len2 = getLength(l2);        
        
        //Making sure l1 is always greater than l2
        if(len2 > len1) {
            ListNode temp = l1;
            l1 = l2;
            l2 = temp;
            
            int t = len1;
            len1 = len2;
            len2 = t;
        }
        
        // Add the linked lists recursively
        int rem = logic(l1, l2, len1, len2, 0, 0);
        

        // After recusive call, if there is carry over value, set as head node
        if(rem != 0) {
            return new ListNode(rem, l1);
        } else {

            // Return the head node
            return l1;
        }
    }

    //l1 and l2 are the list length respectively, s1 and s2 are the current index in respect to l1 and l2 
    public int logic(ListNode l1, ListNode l2, int len1, int len2, int s1, int s2) {
        // Basecase: Return None and carry value of 0
        if(l1 == null) return 0;
        int diff = len1 - len2;
        
        // Get Current Sum and Current Carry Value to recursively pass back to calling function
        if(diff != 0 && s2 < diff) {
			//case where l2 is smaller than l1, hence we need to skip l2 pointer incrementation
            int rem = logic(l1.next, l2, len1, len2, s1+1, s2+1);
            int sum = l1.val + rem;
            l1.val = sum%10;
            return sum/10;
        } else {
            int rem = logic(l1.next, l2.next, len1, len2, s1+1, s2+1);
            int sum = l1.val + l2.val + rem;
            l1.val = sum%10;
            return sum/10;
        }
    }

    //Utility method to get the length of the list
    public int getLength(ListNode head) {
        int length = 0;
        while(head != null) {
            length++;
            head = head.next;
        }
        return length;
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the linked list 1. `M` represents the number of nodes in the linked list 2. 

* **Time Complexity**: `O(N or M)` because we need to make N or M recursive calls to add the all digits. Constant Time. 
* **Space Complexity**: `O(1)` not counting the memory stack, because the maximum number of recursive calls we need to store in the memory stack is based on `O(N or M)`