## Problem Highlights

* 🔗 **Leetcode Link:** [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Linked List, Dummy Node
* 🗒️ **Similar Questions**: [Add Binary](https://leetcode.com/problems/add-binary/), [Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/), [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)
    
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
   
```markdown
HAPPY CASE
Input: 2->4->3, 5->6->4
Output: 7->0->8

Input: 0, 0
Output: 0

EDGE CASE
Input: 9->9->9->9->9->9->9, 9->9->9->9
Output: 8->9->9->9->0->0->0->1

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

**⚠️ Common Mistakes**

- Which of the above would provide us with an optimal solution?


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Iterate through both lists and sum the values of the nodes and remainder from the previous addition for each new node.


```markdown
1) Create a dummy head. This will be our reference to our return list.
2) Create a curr pointer that helps us build the return list
3) Initialize a variable to store the remainder value, if any, as we compute the sum. 
4) Traverse the two lists while our two pointers is not null and remainder is not 0.
    a) Find the values at each pointer
    b) Find and store their sum
    c) Calculate the carry over value, if any
    d) Create and attach a new node with summed value to the return list.
    e) Repeat with next nodes
5) Return dummy.next
```
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        # Create a dummy head. This will be our reference to our return list + Create a curr pointer that helps us build the return list
        dummyNode = curr = ListNode()
        
        # Initialize a variable to store the remainder value, if any, as we compute the sum. 
        remainder = 0
        
        # Traverse the two lists while our two pointers is not null and remainder is not 0.
        while l1 or l2 or remainder:
            
            # Find the values at each pointer
            num1 = l1.val if l1 else 0
            num2 = l2.val if l2 else 0
            
            # Find and store their sum
            total = num1 + num2 + remainder
            singleDigitTotal = total % 10
            
            # Calculate the carry over value, if any
            remainder = total // 10 
            
            # Create and attach a new node with summed value to the return list
            curr.next = ListNode(singleDigitTotal)
            
            # Repeat with next nodes
            curr = curr.next
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next
                
        # Return dummy.next
        return dummyNode.next
```
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // Create a dummy head. This will be our reference to our return list.
        ListNode dummyHead = new ListNode(0);
        
        // Create a curr pointer that helps us build the return list
        ListNode curr = dummyHead;
        
        // Initialize a variable to store the remainder value, if any, as we compute the sum
        int remainder = 0;
        
        // Traverse the two lists while our two pointers is not null and remainder is not 0
        while (l1 != null || l2 != null || remainder != 0) {
            
            // Find the values at each pointer
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            
            // Find and store their sum
            int sum = remainder + x + y;
            
            // Calculate the carry over value, if any
            remainder = sum / 10;
            
            // Create and attach a new node with summed value to the return list.
            curr.next = new ListNode(sum % 10);
            
            // Repeat with next nodes
            curr = curr.next;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        
        // Return dummy.next 
        return dummyHead.next;
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

* **Time Complexity**: `O(N + M)` because we  need to traverse all numbers in both linked list
* **Space Complexity**: `O(N or M)` because the maximum number of digit we need to store is the number of digits in the longer list plus one more digit. I.E. 999 + 999 = 8991 or 1 + 99 = 001