## Problem Highlights

* 🔗 **Leetcode Link:** [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Binary Trees, Binary Search Tree, 
* 🗒️ **Similar Questions**: [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/), [Closest Nodes Queries in a Binary Search Tree](https://leetcode.com/problems/closest-nodes-queries-in-a-binary-search-tree/), [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What is the type of tree we are working with?
  - The input will be a binary search tree
- Can I assume the tree will be complete?
  - No. A node may have less than 2 children
- Can I expect to receive an empty tree as input?
  - No, the input tree can have 1-10000 nodes
- What is the time and space constraints for this problem?
    - `O(n)` time and `O(1)` space complexity without the recursive stack.
```markdown
HAPPY CASE
Input: root = [2,1,3]
Output: true
```
![Example 1](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)
```markdown
Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```
![Example 2](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)
```markdown
EDGE CASE
Input: root = [1]
Output: true
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Since we need to identify the a valid binary search tree, sorted order can help. An in-order traversal will help us retrieve the nodes in sorted order and thereby verify that the Binary Search Tree is indeed valid. 
- Store nodes within a HashMap to refer to later
    - We don't need to access the nodes or their values after they have been processed, so this technique is not useful for this problem
- Using Binary Search to find an element
    - We cannot use binary search for this problem because this does not help us identity the sorted order of the item in question.
- Applying a level-order traversal with a queue
    - A level-order traversal is not helpful here. The level-order traversal does not help us with the sorted order.
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Recursive pre-order traversal to help valid each node. A valid node is between the maximum value and minimum value determined by parent nodes.

```markdown
1) Create a helper function that processes tree via pre-order traversal while retaining the maximum value and minimum value of node as determined by parent nodes.
    a) Base Case: Check if the root is None. If it is, return true. We made it to the leaf of a tree and every node along the path is valid.
    b) Check if the node is within minimum and maximum range. If it is not, return false. 
    b) Call helper on left child, when going left, update the maximum value of child node is less than parent node
    c) Call helper on right child, when going right, update the minimum value of child node is greater than parent node
2) Call helper function on root and set minimum value of node to be negative infinite and maximum value of node to be positive infinite
```

**⚠️ Common Mistakes**
- Not recognizing that the input is a binary search tree
    - Read the problem description carefully and clarify with your interviewer if you see this (or a different tree problem) in an interview
- Choosing the incorrect traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary
- Not recognizing the need for a helper function
    - To help increase the number of parameters.
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        # Create a helper function that processes tree via pre-order traversal while retaining the maximum value and minimum value of node as determined by parent nodes.
        def helper(node: Optional[TreeNode], minimumValueOfNode: int, maximumValueOfNode: int) -> bool:
            # Check if the node is None. If it is, return true. We made it to the leaf of a tree and every node along the path is valid.
            if not node:
                return True
            
            # Check if the node is within minimum and maximum range. If it is not, return false.
            if node.val <= minimumValueOfNode or node.val >= maximumValueOfNode:
                return False
            
            # Call helper on left child, when going left, update the maximum value of child node is less than parent node
            # Call helper on right child, when going right, update the minimum value of child node is greater than parent node
            return helper(node.left, minimumValueOfNode, node.val) and helper(node.right, node.val, maximumValueOfNode)

        # Call helper function on root and set minimum value of node to be negative infinite and maximum value of node to be positive infinite
        return helper(root, -math.inf, math.inf)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in binary search tree.
    
* **Time Complexity**: O(N)
    *  We only need to visit each node along the path of the binary search tree and verify each node's validity.
* **Space Complexity**: O(1) 
    * O(1) when we do not account for recursive stack, but O(N) for space if we account for a recursion stack on a tree with a linked list type structure.