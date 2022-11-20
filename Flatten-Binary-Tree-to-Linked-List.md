## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/flatten-binary-tree-to-linked-list/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Binary Trees
* 🗒️ **Similar Questions**: []()
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do I construct the linked list using tree nodes?
  - You should construct the linked list such that the "right" pointer of the tree node will be used as the "next" pointer for a linked list"
- What about the left pointer?
  - All left pointers should be set to NULL
   
```markdown
HAPPY CASE

Input: root = [1,2,5,3,4,null,6]
Output: [1,null,2,null,3,null,4,null,5,null,6]

Input: root = [1,3,2,5,3,null,9]
Output: [1,3,9]

Input: root = [1,2,3]
Output: [1,3]


EDGE CASE
Input: root = []
Output: []

Input: root = [0]
Output: [0]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
  - We can visit & flatten the left subtree of a node, visit & flatten the right subtree of a node, then connect them somehow
Since we are visiting left, visiting right, then doing something, it seems like this will utilize a post-order traversal
We need to figure out what information we need to keep track of to be able to connect the flatten subtrees
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1) Traverse the tree via a post-order traversal
2) When we visit a node, recursively flatten its left subtree (if it exists), then its right subtree (if it exists)
3) To correctly arrange the flattened subtrees, the current node's next pointer (in this case we use the right pointer as a next pointer) 
should be set to the head of the left flattened subtree, then the tail of the left flattened subtree should be set to the head of the right flattened subtree
    - There are some special cases to consider:
        - If there is no right subtree, then the tail of the left subtree should be connected to NULL
        - If there is no left subtree, the current node should be connected directly to the head of the right flattened subtree
          (Luckily for us, this is how the tree is already setup, so no action needs to be taken in this case.)
        - If there is neither a left nor right subtree, then the current node should be connected to NULL
          (This is how the tree is already setup when there are no subtrees at the current node, so again no action needs to be taken.)
4) Based on step 3, we need to be able to keep track of the head and tail of a flattened portion of the tree, and return them up the call stack as we recurse
    - The head of the flattened subtree will be the current node we are visiting
    - The tail of the flattened subtree will vary depending on what subtrees were visited:
        - If there was a right subtree, the tail will be the tail of the right flattened subtree
        - If there was no right subtree but there was a left subtree, the tail will be the tail of the left flattened subtree
        - If there was neither a left nor right subtree, the tail will just be the current node
```

**⚠️ Common Mistakes**

* Make sure to use a post-order traversal. We cannot flatten the subtree rooted at a node until we have flattened its left and right subtrees first.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    // stores root of branch that was previously flattened
    private TreeNode prevRoot = null;

    // recursively flattens using post-order traversal
    // visits each node once: O(n) time, O(h) space
    public void flatten(TreeNode root) {
        if (root == null) return;

        // flattens subtrees in post-order
        // this ensures flattened right sub-list comes after the left sub-list
        flatten(root.right);
        flatten(root.left);

        // after children are flattened, just set head of sub-list as child
        root.right = prevRoot;
        root.left = null;

        prevRoot = root
    }
}
```
```python
def flatten(root: Optional[TreeNode]) -> None:
        def getList(node):
            # initialize head and tail of left flattened subtree to None
            leftHead = None
            leftTail = None

            # if the left subtree exists, recursively flatten and get the
                # head and tail of the flattened subtree
            if node.left:
                (leftHead, leftTail) = getList(node.left)
            
            # initialize head and tail of right flattened subtree to None
            rightHead = None
            rightTail = None

            # if the right subtree exists, recursively flatten and get the
                # head and tail of the flattened subtree
            if node.right:
                (rightHead, rightTail) = getList(node.right)
            
            # we have variables pointing to the left and right subtrees, so we can
                # safely set the node's left to None
            node.left = None

            # if there is a left subtree, we need to set the node's next (right) pointer
                # to point to the head of the flattened left subtree
                # We also need to set the tail of the left flattened subtree to be the head
                # of the right flattened subtree.
                # (Note: since we initialized rightHead to None above, this will give the
                # correct assignment even if there is no right subtree)
            if leftHead:
                node.right = leftHead
                leftTail.right = rightHead
            
            # based on the presence of flattened subtrees, we determine the head and tail of the
                # flattened portion of the tree
                # - The head will always be the current node
                # - The tail will either be the rightTail, leftTail, or the current node itself,
                #   depending on what is defined
            if rightHead:
                return (node, rightTail)
            elif leftHead:
                return (node, leftTail)
            else:
                return (node, node)
        
        # handle null root edge case
        if not root:
            return root
        
        # return the head of the flattened tree
        return getList(root)[0]
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(N)
* Space Complexity: O(1)