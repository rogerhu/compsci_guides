## Problem Highlights

* 🔗 **Leetcode Link:** [Convert Sorted List to BST](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Binary Trees, Binary Search Tree, Linked List
* 🗒️ **Similar Questions**: [Path Sum II](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/), [Smallest String Starting From Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf/), [Step-By-Step Directions From a Binary Tree Node to Another](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input node be Null?
  - Yes, it can be. In that case, we don’t generate any Tree and return Null.

```markdown
HAPPY CASE
Input: [-10,-3,0,5,9]
Output:   0
         / \
       -3   9
       /   /
     -10  5

Input: head = [0]
Output: [0]

EDGE CASE
Input: head = []
Output: []
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - It is very important to identify which tree traversal to use in this situation. As the LinkedList is in ascending order, which means we are iterating through a sorted list, such a tree traversal would also have to be ascending in a BST. This means we should be targeting an In-Order tree traversal.
- Store nodes within a HashMap to refer to later
    - Since the nodes are sorted and there aren’t any reasons to refer to earlier nodes with a specific key, a HashMap would not help.
- Using Binary Search to find an element
    - We are not working with a Binary Search Tree. 
- Applying a level-order traversal with a queue
    - Using this approach may complicate our code

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** This sorted linked list is also an inorder traversal. Find the mid of the array. Use it as root. Recursively call buildBST on the left nodes and right nodes and add them to the root.

```markdown
1) Determine the size of the LL
2) Keep a global reference to a head variable that can change
3) Trigger Helper function with paramters 0 and size - 1 to indicate current scope
4) Return Helper function value

Helper
1) If the current L, R pointers are inverted, return
2) Calculate the midpoint of the L, R pointers
3) Recurse left (as an In-Order Traversal would) with params L and Mid - 1
4) Create a node with the current head node value and move the head pointer forward
5) Recurse right with params Mid + 1 and R
6) Attach left sub-tree from left recursion to current node
7) Attach right sub-tree from right recursion to current node
8) Return current node
```

**⚠️ Common Mistakes**
- Not noticing or clarifying that the input list is sorted
    - Not recognizing how this sorted property allows us to build the BST
- Choosing the incorrect traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary
    - Suppose you attempted to perform level-order traversal, how would you know each layer.
    - Suppose you chose to perform pre-order or post-order, you are ignoring the ordering of a BST.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def helper(start, end):
            # Basecase: If the current start and end pointers are inverted, return
            if start > end:
                return
            global h

            # Calculate the midpoint of the start and end pointers
            mid = (start + end) // 2

            # Recurse left (as an In-Order Traversal would) with params L and Mid - 1
            left = helper(start, mid - 1)

            # Create a node with the current head node value and move the head pointer forward
            cur_node = TreeNode(h.val)
            h = h.next

            # Recurse right with params Mid + 1 and R
            right = helper(mid + 1, end)

            # Attach left sub-tree from left recursion to current node and Attach right sub-tree from right recursion to current node
            cur_node.left, cur_node.right = left, right

            # Return current node
            return cur_node
                
        def find_size(root):
            ret = 0
            while root:
                ret += 1
                root = root.next
            return ret

        # Determine the size of the LL
        size = find_size(head)

        # Keep a global reference to a head variable that can change
        global h
        h = head

        # Trigger Helper function with paramters 0 and size - 1 to indicate current scope and Return Helper function value
        return helper(0, size - 1)
```
```java
class Solution {
    ListNode head;
    public TreeNode sortedListToBST(ListNode head) {
      // Keep a global reference to a head variable that can change
      this.head = head;
      // Trigger Helper function with paramters 0 and size - 1 to indicate current scope and Return Helper function value
      return buildBST(0, length(head) - 1);
    }

    TreeNode buildBST(int left, int right) {
      // Basecase: If the current start and end pointers are inverted, return
      if (left > right) return null;

      // Calculate the midpoint of the start and end pointers
      int mid = (left + right) / 2;

      // Recurse left (as an In-Order Traversal would) with params L and Mid - 1
      TreeNode leftNode = buildBST(left, mid - 1);
      
      // Create a node with the current head node value and move the head pointer forward
      TreeNode root = new TreeNode(head.val); 
      head = head.next; 
      
      // Recurse right with params Mid + 1 and R
      // Attach left sub-tree from left recursion to current node and Attach right sub-tree from right recursion to current node
      root.left = leftNode;
      root.right = buildBST(mid + 1, right);

      // Return current node
      return root;
    }

    int length(ListNode head) {
        int ans = 0;
        while (head != null) {
            head = head.next;
            ans++;
        }
        return ans;
    }
}
```    

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in array 
    
* **Time Complexity**: O(N)
    *  We need to visit each node in the Linked List to build binary search tree.
* **Space Complexity**: O(logN) 
    * O(logN) because we need O(logN) space to represent the system recursion stack. Otherwise O(1) because we are reusing the nodes from the LinkedList.