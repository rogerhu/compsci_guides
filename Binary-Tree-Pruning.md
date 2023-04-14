## Problem Highlights

* 🔗 **Leetcode Link:** [Binary Tree Pruning](https://leetcode.com/problems/binary-tree-pruning/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Binary Trees, Depth First Search
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What should I output?
  - Your program should output the same tree given as input, with all subtrees that do not have a 1 removed.
- Can the root node be NULL?
  - No, the input tree will always have at least 1 node
- What conditions need to be satisfied for a subtree to warrant removal?
- How do we check whether a subtree needs to be removed? What kind of traversal works best for this checking?
- Consider under what conditions a leaf node should be removed? What about a subtree of one node with two children? How do we check that it should be removed or not?
   
```markdown
HAPPY CASE
Input:              [0]
               /            \
             [1]            [1]
          /      \        /       \
        [0]      [1]    [0]       [0]
      /         /     /     \        \
     [1]       [0]   [0]    [0]      [1]   
Output:             [0]
               /            \
             [1]            [1]
          /      \              \
        [0]      [1]            [0]
      /                             \
     [1]                              [1]  



Input:              [1]
               /            \
             [1]            [1]
          /      \        /       \
        [0]      [1]    [0]       [0]
      /         /     /     \        \
     [1]       [0]   [0]    [0]      [1]   
Output:             [1]
               /            \
             [1]            [1]
          /      \              \
        [0]      [1]            [0]
      /                             \
     [1]                              [1]  


EDGE CASE
Input:              [0]
               /            \
             [0]            [0]
          /      \        /       \
        [0]      [0]    [0]       [0]
      /         /     /     \        \
     [0]       [0]   [0]    [0]      [0]  
Output: Since there are no 1's in the entire tree, we should remove all nodes. 
In this case, we should return NULL (this would be a good clarifying question to ask your interviewer).
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
  - For each node, we need to determine if it's the root of a subtree containing at least one 1
To fulfill that condition, either it's value needs to be 1, or either it's left or right subtree needs to contain a 1
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

Traverse the tree via DFS, recursively prune left and right subtrees, then return information regarding whether or not current subtree should be pruned.

```markdown
1) Traverse the tree via DFS
2) As we traverse, we want to determine if the current node is the root of a subtree containing a 1
3) We can recursively prune the left and right subtrees of the current node, 
and then return information about whether or not the pruned left/right subtree contains a 1 back to the current node
4) If the left/right subtree does not contain a 1, 
it should be removed from the tree (by setting the current node's left/right child to NULL)
5) Then, if the node's value is 1 or one of its subtrees contains a 1, 
we know to return true back to the parent node
```

**⚠️ Common Mistakes**

* Make sure to use a post-order traversal. We cannot prune a subtree until we know for certain that there is not a 1 at any point down the tree. For example, consider a tree of all 0's except the leaves, which are all 1's. In this case, we wouldn't remove any part of the tree since every subtree contains at least one leaf, and all the leaves have value 1. The only way we can verify this before pruning is to recursively visit the children nodes before deciding to prune or not.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    public TreeNode pruneTree(TreeNode root) {
        return containsOne(root) ? root : null;
    }

    public boolean containsOne(TreeNode node) {
        if (node == null) return false;
        
        // check if any node in the left subtree contains a 1.
        boolean leftContainsOne = containsOne(node.left);
        
        // check if any node in the right subtree contains a 1.
        boolean rightContainsOne = containsOne(node.right);

        // if the left subtree does not contain a 1, prune the subtree.
        if (!leftContainsOne) node.left = null;
        
        // if the right subtree does not contain a 1, prune the subtree.
        if (!rightContainsOne) node.right = null;
        
        // return true if the current node, its left or right subtree contains a 1.
        return node.val == 1 || leftContainsOne || rightContainsOne;
    }
}
```
```python
def pruneTree(self, root):
    def pruneHelper(node):

        # assume that leftSubTree does not contain 1
        leftSubTreeContains1 = False

        # if there is a left child, then the left subtree could have a 1, so recursively check this
        if node.left:
            leftSubTreeContains1 = pruneHelper(node.left)
        
        # assume that rightSubTree does not contain 1
        rightSubTreeContains1 = False

        # if there is a right child, then the right subtree could have a 1, so recursively check this
        if node.right:
            rightSubTreeContains1 = pruneHelper(node.right)

        # if the leftSubTree does not have a 1, it should be removed, as per the algorithm specification
        if not leftSubTreeContains1:
            node.left = None

        # if the rightSubTree does not have a 1, it should be removed, as per the algorithm specification
        if not rightSubTreeContains1:
            node.right = None

        # the subtree rooted at this node contains a 1 if:
            # 1) the root of the subtree (the current node) has value 1
            # 2) the leftSubTree contains a 1
            # 3) the rightSubTree contains a 1
        # The parent node will use this return value to determine if its child should be pruned or not
        return node.value == 1 or leftSubTreeContains1 or rightSubTreeContains1
    
    # calling pruneHelper on the root tells us if the tree has a 1 or not
    treeHas1 = pruneHelper(root)

    # if the tree does not contain a 1, then all nodes (including the root) should be removed
    if not treeHas1:
        return None
    
    # otherwise, return the pruned tree
    return root
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(n), where n is the number of nodes in the tree. We process each node once.

* Space Complexity: O(n), the recursion call stack can be as large as the height of the tree. 