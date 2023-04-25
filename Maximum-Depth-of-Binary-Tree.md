## Problem Highlights

* 🔗 **Leetcode Link:** [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Binary Trees, Depth First Search
* 🗒️ **Similar Questions**: [Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/), [Symmetric Tree
](https://leetcode.com/problems/symmetric-tree/), [Same Trees](https://leetcode.com/problems/minimum-depth-of-binary-tree/), [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What is the type of tree we are working with?
  - The input tree will be a binary tree
- Can I assume the tree will be complete?
  - No. A node may have less than 2 children
- Can I expect to receive an empty tree as input?
  - Yes, the input tree can have 0-10000 nodes.
   
```markdown
HAPPY CASE
Input: root = [3,9,20,null,null,15,7]
Output: 3
```

![Example 1](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

```markdown
Input: root = [1,null,2]
Output: 2

EDGE CASE
Input: root = []
Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Since we need to get the height of the tree, we will need to traverse all nodes down the tree, we should recursively return depth of each node upward to parent. a Pre-Order traversal would be appropriate
- Store nodes within a HashMap to refer to later
    - We don't need to access the nodes or their values after they have been processed, so this technique is not useful for this problem
- Using Binary Search to find an element
    - We are not looking to find an element in this problem so this technique is not useful for this problem
- Applying a level-order traversal with a queue
    - Since we need to find the height of the tree, we could get nodes by level. a level-order traversal using a queue would be appropriate
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Recursively ask for child height in tree and add one for parent.

```markdown
1. Basecase is a Null Node, return 0
2. Recursively return the max between height of left node and right node and add one for current node.
```

**⚠️ Common Mistakes**
- Choosing the wrong traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary
    - Suppose you choose to perform Level order traversal, yes that would work, but how long is your code.

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
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # Basecase is a Null Node, return 0
        if not root:
            return 0

        # Recursively return the max between height of left node and right node and add one for current node.
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```
```java
class Solution {
    public int maxDepth(TreeNode root) {
        // Basecase is a Null Node, return 0
        if(root == null)  return 0;
        // Recursively return the max between height of left node and right node and add one for current node.
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;    
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in binary tree 
    
* **Time Complexity**: `O(N)`, because we need to visit each node in the binary tree
* **Space Complexity**: `O(N)` because we need to account for unbalanced tree (space is used for the recursion stack). In the general case `O(logN)` for balanced tree.