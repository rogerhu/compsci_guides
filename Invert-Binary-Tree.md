## Problem Highlights

* 🔗 **Leetcode Link:** [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Binary Trees, Depth First Search, Breadth First Search
* 🗒️ **Similar Questions**: [Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/), [Symmetric Tree
](https://leetcode.com/problems/symmetric-tree/), [Same Trees](https://leetcode.com/problems/minimum-depth-of-binary-tree/) 
    
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
  - Yes, the input tree can have 0-100 nodes.
   
```markdown
HAPPY CASE
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

![Example 1](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```markdown
Input: root = [2,1,3]
Output: [2,3,1]
```

![Example 2](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```markdown
EDGE CASE
Input: root = []
Output: []
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Since we need to swap nodes all the way down the tree, we should recursively swap nodes. a Pre-Order traversal would be appropriate
- Store nodes within a HashMap to refer to later
    - We don't need to access the nodes or their values after they have been processed, so this technique is not useful for this problem
- Using Binary Search to find an element
    - We are not looking to find an element in this problem so this technique is not useful for this problem
- Applying a level-order traversal with a queue
    - Since we need to swap nodes, we could swap each node by level. a level-order traversal using a queue would be appropriate

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Recursively swap left node for right node.

```markdown
1. Basecase is a Null Node
2. Swap the left and right nodes
3. Return the node with swapped children.
```

**⚠️ Common Mistakes**
- Choosing the wrong traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary
    - Suppose we chose level order traversal, how complex would that be?

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
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # Basecase is a Null Node
        if not root:
            return None

        # Swap the left and right nodes
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)

        # Return the node with swapped children
        return root
```
```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        // Basecase is a Null Node
        if(root == null){
            return root;
        }
        // Swap the left and right nodes
        invertTree(root.left);
        invertTree(root.right);
        TreeNode curr = root.left;
        root.left = root.right;
        root.right = curr;

        // Return the node with swapped children
        return root;        
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