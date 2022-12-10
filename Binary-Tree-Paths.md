## Problem Highlights

* 🔗 **Leetcode Link:** [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Binary Trees, Depth-First-Search
* 🗒️ **Similar Questions**: [Path Sum II](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/), [Smallest String Starting From Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf/), [Step-By-Step Directions From a Binary Tree Node to Another](https://leetcode.com/problems/step-by-step-directions-from-a-binary-tree-node-to-another/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input tree be Null?
  - Yes, it can be. In that case there are no root-to-leaf paths, correct?
- Can a root-to-leaf path be the root itself, if it has no children?
  - Yes, that can occur.
```markdown
HAPPY CASE
Input:     1
         /   \
        2     3
         \
          5

Output: ["1->2->5", "1->3"]

Input:     1
         /   \
        2     3
         \     \
          5     2

Output: ["1->2->5", "1->3->2"]

EDGE CASE (Only Root Node)
Input:  1
Output: ["1"]

```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Choosing a specific tree traversal that follows a general root-to-leaf path should help us identify all of the possible routes.
    
- Store nodes within a HashMap to refer to later
    - We don’t have a specific way of referring to previous nodes in a path that could be used in a HashMap. So, a HashMap would not help us as much in this context.

- Using Binary Search to find an element
    - We are not working with a Binary Search Tree. 

- Applying a level-order traversal with a queue
    - Using this approach may complicate our code
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Pre-Order traversal through the Binary Tree while keeping track of a current path. When we reach a leaf node, store the current path and pop elements off as we backtrack and explore more of the tree.

```markdown
0) Create helper function to allow us to retain memory of allPaths
1) Basecase: If the current element is Null, return
2) Recusive: Build Current Path 
    a) Add the current node to the current path data structure
    b) If the current node is a leaf node, store the current path in the tree
    c) Recurse Left and Recurse Right
    d) Pop the current node off the current path, since we will no longer have more root-to-leaf paths that go through the current node.
3) Create allPaths array to retain memory of allPaths
4) Run helper function to collect allPaths
5) Return allPaths
```

**⚠️ Common Mistakes**
- Not noticing the need for a helper function to retain memory 
- Not noticing the need to clear current path after recursion a.k.a backtracking

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
        
        # Create helper function to allow us to retain memory of allPaths
        def helper(root, currPath):
            # Basecase: If the current element is Null, return
            if not root:
                return

            # Recusive: Build Current Path 
            # Add the current node to the current path data structure
            currPath.append(str(root.val))

            # If the current node is a leaf node, store the current path in the tree
            if not root.left and not root.right:
                allPaths.append("->".join(currPath))

            # Recurse Left and Recurse Right
            helper(root.left, currPath)
            helper(root.right, currPath)
            
            # Pop the current node off the current path, since we will no longer have more root-to-leaf paths that go through the current node.
            currPath.pop()

        # Create allPaths array to retain memory of allPaths
        allPaths = []

        # Run helper function to collect allPaths
        helper(root, [])

        # Return allPaths
        return allPaths
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in array 
    
* **Time Complexity**: O(N)
    *  We need to visit each node in the Binary  Tree.
* **Space Complexity**: O(N) 
    * O(N) because we need O(N) space to represent each nodes in the binary tree. 