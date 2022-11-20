## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/minimum-depth-of-binary-tree/>
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Depth First Search, Breadth First Search, Binary Trees
* 🗒️ **Similar Questions**: [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/), [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

* **Q:** What exactly is the input?
  * **A:** "The input is a root node of the binary tree we want to find the min depth of"
* **Q:** Can the root node be NULL? If so, what would the min-depth be?
  * **A:** "Yes, in this case, the min-depth would be 0" 
   
```markdown
HAPPY CASE
Input: [1, 2, 3, 4, null, 5, 6, null, null, 7, 8, null, 9]
                  [1]
                /       \
              [2]       [3]
              /      /       \
            [4]    [5]       [6]
                 /     \        \
                [7]    [8]      [9]   
Output: 3 ([1] -> [2] -> [4] path has 3 nodes)

Input: root = [3,9,20,null,null,15,7]
Output: 2

EDGE CASE
Input:          [6]
Output: 1 (root is a leaf, so path from root to leaf goes through 1 node)
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**Approach #1:** Traverse the tree via DFS, keeping track of the current depth in the tree. When we find a leaf, update the minimum depth and return min depth at the end.

```markdown
1) Traverse the tree via DFS
2) As we traverse, keep track of the current level in the tree, starting with level 0 for the root node
3) Every time we visit a left/right child of the current node, increment the level by 1
4) If we visit a node with no children, it is a leaf, and its depth is its level + 1. Track the depths of discovered leaves.
5) Return the minimum of all leaf depths.

Time Complexity: O(N) since we visit all nodes in the tree
Space Complexity: O(1) if tracking only the minimum depth found so far, O(N) if we store all depths

Cons:
- We have to visit every node in the tree because we are using DFS (the min-depth root-to-leaf path might be the last one we traverse)
```

**Approach #2:**Traverse the tree via BFS (level-by-level), keeping track of current depth in the tree. Return depth of the first leaf node encountered.

```markdown
1) Traverse the tree via BFS (using a queue)
2) As we traverse, keep track of current level in the tree, starting with level 0 for the root node
3) Once we find a leaf, return its depth

Time Complexity: O(N) since we (in the worst case) visit all nodes in the tree
Space Complexity: O(1) (not including space needed for queue)

Cons:
Have to use an auxiliary data structure (queue) to process nodes
```

**⚠️ Common Mistakes**

* A tree consisting of only a root node has min-depth 1, because the shortest root-to-leaf path consists of 1 node (the root itself)
* Be careful when doing the traversal to maintain the proper level of each node. Every time we visit a new node, we go up one level in the tree, since every child is one layer lower in the tree than its parent

## 4: I-mplement

> **Implement** the code to solve the algorithm.

Approach #1: DFS

```python
#Approach 1: DFS

def minDepth(self, root):
    """
    :type root: TreeNode
    :rtype: int
    """
    # a tree with no nodes has min depth 0
    if root == None:
        return 0

    # create a queue, and initialize it with the root node that has depth 1
    queue = deque()
    queue.append((root, 1))

    # BFS traversal
    while len(queue) > 0:
        curr = queue.popleft()
        currNode, currLevel = curr[0], curr[1]

        # BFS traversals go level-by-level, so the first leaf found will be at the highest level of any leaf, and thus will have the min depth of any leaves
            # therefore, we can return its level right away
        if currNode.left == None and currNode.right == None:
            return currLevel

        # if the node is not a leaf, continue visiting its children, which are at level = currLevel + 1
        if currNode.left != None:
            queue.append((currNode.left, currLevel + 1))
        if currNode.right != None:
            queue.append((currNode.right, currLevel + 1))
    return -1
```

**Approach #2: BFS**

```python
#Approach 2: BFS

def minDepth(self, root):
    """
    :type root: TreeNode
    :rtype: int
    """
    # this helper function will find the depth of all leaf nodes in the tree
        # if the current node is not a leaf, it will continue traversal, incrementing the current depth we are at
    def minDepthHelper(node, cur_depth) -> int:
        # if the node is a leaf, its min depth is the current depth
        if not node.left and not node.right:
            return cur_depth
        
        # visit the left child to look for leaves, incrementing current depth by 1
        left_depth = float('inf')
        if node.left:
            left_depth = minDepthHelper(node.left, cur_depth+1)

        # visit the right child to look for leaves, incrementing current depth by 1 
        right_depth = float('inf')
        if node.right:
            right_depth = minDepthHelper(node.right, cur_depth+1)
        
        # return the minimum leaf depth found between left paths and right paths
        return min(left_depth, right_depth)
    
    # a tree with no nodes has min depth 0
    if not root:
        return 0
    
    # start traversal at the root, which has depth = 1
    return minDepthHelper(root, 1)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity – Best Case: O(n)
* Space Complexity – Best Case: O(h) for recursion stack