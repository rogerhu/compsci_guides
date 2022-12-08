## Problem Highlights

* 🔗 **Leetcode Link:** [Same Trees](https://leetcode.com/problems/minimum-depth-of-binary-tree/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Binary Trees, Depth First Search, Breadth First Search
* 🗒️ **Similar Questions**: [Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/), [Symmetric Tree
](https://leetcode.com/problems/symmetric-tree/), [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
    
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
Input: p = [1,2,3], q = [1,2,3] 
Output: true

Input: p = [1,2], q = [1,null,2]
Output: false

EDGE CASE
Input: p = [], q = []
Output: true
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Since we need to compare nodes and node values in both trees top down, a Pre-Order traversal would be appropriate
- Store nodes within a HashMap to refer to later
    - We don't need to access the nodes or their values after they have been processed, so this technique is not useful for this problem
- Using Binary Search to find an element
    - We are not looking to find an element in this problem so this technique is not useful for this problem
- Applying a level-order traversal with a queue
    - Since we need to compare nodes and node values in both trees top down, a level-order traversal using a queue would be appropriate
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Recursively traverse both trees using Pre-Order traversal and check if each node is equal to the other.

```markdown
1. Recursively traverse the tree using Pre-Order traversal
    a. If the nodes from both trees are not None, and their values are equal -> Recur on their children
    b. Else -> return False
2. After trees have been fully traversed and no differences are discovered, return True
```

**General Idea:** Level Order Traversal With Queue and check if each node is equal to the other.

```markdown
1) Initialize queues used to traverse both trees 
2) Push root of respective trees onto the queue, given that they are not None
3) While the queue(s)are not empty:
    a) Pop a node from the front of each queue
    b) If their values are equal -> Push their children to the end of the queue
    c) Else -> return False
4) Once the trees are traversed and no differences are discovered -> return True
```

**⚠️ Common Mistakes**
- Choosing the wrong traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # After trees have been fully traversed and no differences are discovered, return True
        if not p and not q:
            return True

        # If the nodes from both trees are not None, and their values are equal -> Recur on their children
        if p and q and p.val == q.val:
            return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
        else:
            return False
```
```python
class Solution:
    def isSameTree(self, p, q): 
        def equalNode(p, q):
            # if both are None
            if not p and not q:
                return True
            # one of p and q is None
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True

        # Initialize queues used to traverse both trees 
        deq = deque()

        # Push root of respective trees onto the queue, given that they are not None
        deq.append((p,q))

        # While the queue(s)are not empty
        while deq:
            # Pop a node from the front of each queue
            p, q = deq.popleft()

            # If their values are equal -> Push their children to the end of the queue
            if equalNode(p, q):
                if p and q:
                    deq.append((p.left, q.left))
                    deq.append((p.right, q.right))

            # Else -> return False
            else:
                return False

        # Once the trees are traversed and no differences are discovered -> return True
        return True
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in binary tree 
    
* **Time Complexity**: O(N)
    *  We need to visit each node in the binary tree
* **Space Complexity**: O(N) 
    * O(N) using a queue because we perform level-order traversal and this may require N/2 nodes to be store in queue.
    * O(N) using recursion, because O(N) for unbalanced tree (space is used for the recursion stack). In the general case O(logN) for balanced tree.