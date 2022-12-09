## Problem Highlights

* 🔗 **Geeksforgeeks Link:**  [Print all leaf nodes of a Binary Tree from left to right](https://www.geeksforgeeks.org/print-leaf-nodes-left-right-binary-tree/)
* **Difficulty:** Easy
* **Time to complete**: 10 mins
* **Topics**: Binary Tree, Pre-Order Traversal
* **Similar Questions**: [Print Binary Tree](https://leetcode.com/problems/print-binary-tree/),[Same Tree](https://leetcode.com/problems/same-tree/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input tree be Null?
    - Yes, the input could be Null. In that case, we do not print anything, since there are no leaf nodes.
- Could the tree possibly have no leaf nodes?
    - Unless the tree is Null, there have to be leaf nodes. Even the root can be a leaf node if it has no children.
```markdown
HAPPY CASE
Input:     1
          / \
         3   4

Output: 3 4

Input:     2
          / \
         4   7
        / \
       8   5

Output: 8 5 7

EDGE CASE
Input:   1
Output: 1 
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Trees, some things we should consider are:
- Some type of Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Choosing a specific Tree Traversal can help us in this problem identify leaf nodes from left to right.
- Store nodes within a HashMap to refer to later
    - We don’t need to refer to earlier nodes in the tree, so a HashMap would not help us as much in this context.
- Using Binary Search to find an element
    - We are not working with a binary search tree here.
- Applying a level-order traversal with a queue
    - This type of tree traversal does not help because we do not want to look at the tree layer by layer.
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Pre-Order Traversal of the Tree. At every node, verify if the node is a leaf or not. If so, print the value and continue.





```markdown
1) Basecase: If the current node is Null, return
2) If the current node has no children, print its value
3) Recurse Left and Recurse Right
```

**⚠️ Common Mistakes**

* Look out to pop and push element to the right Stack
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def print_leaf_nodes(self, root: TreeNode) -> None:
    # Basecase: If the current node is Null, return
    if not root:
        return
    
    # If the current node has no children, print its value
    if not root.left and not root.right:
        print(root.val, end = " ")
        
    # Recurse Left and Recurse Right
    self.print_leaf_nodes(root.left)
    self.print_leaf_nodes(root.right)
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the in the binary tree. `H` represents the height of the binary tree

* **Time Complexity**: `O(N)` because we will need to access each node in the binary tree.
* **Space Complexity**: `O(H)` because we only need space to hold the recursive stack, which is dependent on the height of the binary tree. 