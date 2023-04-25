## Problem Highlights

* 🔗 **Leetcode Link:** [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Binary Trees,Binary Search Tree
* 🗒️ **Similar Questions**: [Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/), [Closest Nodes Queries in a Binary Search Tree](https://leetcode.com/problems/closest-nodes-queries-in-a-binary-search-tree/), 
    
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
  - Yes, the input tree can have 0-10000 nodes
- Are the node values unique?
    - Yes, the node values are unique
- Is it guaranteed that the value to insert does not exist in the BST?
    - Yes, the node to insert does not originally exist in the BST

```markdown
HAPPY CASE
Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]

Input: root = [40,20,60,10,30,50,70], val = 25
Output: [40,20,60,10,30,50,70,null,null,25]

EDGE CASE
Input: root = [4,2,7,1,3,null,null,null,null,null,null], val = 5
Output: [4,2,7,1,3,5]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Since we need to the value to insert with existing node values in both trees top down, a Pre-Order traversal would be appropriate
- Store nodes within a HashMap to refer to later
    - We don't need to access the nodes or their values after they have been processed, so this technique is not useful for this problem
- Using Binary Search to find an element
    - We are looking to find the correct position at which to insert the value in this problem so this technique is useful for this problem
- Applying a level-order traversal with a queue
    - Since we need to compare nodes and node values in both trees top down, a level-order traversal using a queue could be used but try solving the problem using Pre-Order first

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Recursively traverse binary search trees for the correct position for the current value, by going left if a larger value is seen or going right if a smaller value is seen. 

```markdown
1) Base Case: Check if the tree is empty. If it is, return a new TreeNode with node value of val
2) Recursively traverse the tree in pre-order fashion
    a) Compare the input value with the node values
        i) If input value > node.val: traverse the left subtree
        ii) Else traverse the right subtree
3) Return the root of the tree
```

**⚠️ Common Mistakes**
- Not recognizing that the input is a binary search tree
    - Read the problem description carefully and clarify with your interviewer if you see this (or a different tree problem) in an interview
- Choosing the incorrect traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def insertIntoBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # Check if the tree is empty. If it is, return a new TreeNode with node value of val
        if not root:
            return TreeNode(val)
        
        # Recursively traverse the tree in pre-order fashion
        if root.val > val:
            # If node.val > input value: traverse the left subtree
            root.left = self.insertIntoBST(root.left, val)
        else:
            # Else traverse the right subtree
            root.right = self.insertIntoBST(root.right, val)
        
        # Return the root of the tree
        return root
```
```java
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        // Check if the tree is empty. If it is, return a new TreeNode with node value of val
        if(root==null)
            return new TreeNode(val);
        
        // Recursively traverse the tree in pre-order fashion

        // If node.val > input value: traverse the left subtree
        if(root.val>val)
            root.left = insertIntoBST(root.left,val);
        // Else traverse the right subtree
        else
            root.right = insertIntoBST(root.right,val);

        // Return the root of the tree
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

Assume `H` represents the height of the binary search tree
    
* **Time Complexity**: O(H)
    *  We only need visit each node along the path of the binary search tree. 
* **Space Complexity**: O(H) 
    * O(H) using recursion, because O(H) for space is used for the recursion stack.