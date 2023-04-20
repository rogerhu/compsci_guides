## Problem Highlights

* 🔗 **Leetcode Link:** [Path Sum III](https://leetcode.com/problems/path-sum-iii/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Binary Trees, Depth-First-Search
* 🗒️ **Similar Questions**: [Path Sum](https://leetcode.com/problems/path-sum/), [Path Sum II](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/), [Smallest String Starting From Leaf](https://leetcode.com/problems/smallest-string-starting-from-leaf/), [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/) 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input tree be Null?
  - Yes, it can be. In that case there are no paths, correct?
- Can a path be the root itself, if it has no children?
  - Yes, that can occur.
- What is the Time and Space Constraints for this problem?
    - `O(N^2)` Time and `O(1)` Space if we do not count the recursion stack. 
    -  And `O(N)` Time and `O(N)` Space.

```markdown
HAPPY CASE
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

![Example 1 ](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```markdown
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3

EDGE CASE 
Input: root = [], targetSum = 0
Output: 0
Explanation: Since the tree is empty, there are no paths.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Binary Trees some common techniques you can employ to help you solve the problem:

- Think about appropriate Tree Traversal: Pre-Order, In-Order, Post-Order, Level-Order
    - Choosing a specific tree traversal that follows a general root-to-leaf path should help us identify all of the possible routes.
- Store nodes within a HashMap to refer to later
    - We don’t have a specific way of referring to previous nodes in a path that could be used in a HashMap. So, a HashMap would not help us as much in this context. But a hashmap could store the previous path sums to be used as starting points.
- Using Binary Search to find an element
    - We are not working with a Binary Search Tree. 
- Applying a level-order traversal with a queue
    - Using this approach may complicate our code

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**SOLUTION 1:**

**General Idea:** Lets build the sum as we progress through the nodes. We take two paths as we progress through each nodes. 1. Remove values from our targetSum to represent traversing through entire tree from root node. 2. We will start with a fresh targetSum to represent a frest start at each individual node. Upon reaching a node, check if value is equal to targetSum. Upon each node we can check if we the targetSum hits zero and increase the number of paths found. 

```markdown
1. Create a helper function to process each node and flag for root node to prevent repeated calls to helper from same node. 
    a. Set basecase to root is None, return
    b. Upon reaching a targetSum of zero, increase number of paths by 1
    c. Recursively progress through entire tree from previous node
    d. Recursive progress through entire tree from current node
2. Create variable to hold number of paths found
3. Call helper function to populate number of paths found
4. Return the number of paths found
```

**SOLUTION 2:**

**General Idea:** Lets build the sum as we progress through the nodes and memorize the past path sums to be reused. We will reuse past path sums as our starting points to achieve our targetSum at the current node. 

```markdown
1. Create a helper function to process each node and store and release past path sums.
    a. Set basecase to root is None, return
    b. Generate current path sum by adding current node
    c. Generate past path sum by subtracting current path sum and target. This is the value we need to get the target. Have we seen it? How many times have we seen it?
    d. Add the number of ways we can get to TargetSum from this node, by checking the number of starting points that will get us to target
    e. Add this path to the cache as part of the pastPathSum
    f. Recursively check left child and right child
    g. Release current path sum upon closing resursive call, because current path sum(Starting Point) will no longer be available.
2. Create variable to hold number of paths found
3. Cache the first possible starting point of zero.
4. Call the helper function to find the number of paths
5. Return the results
```

**⚠️ Common Mistakes**
- Choosing the wrong traversal type
    - Try to walk through the problem by hand and see the order in which you are processing the nodes. This will clue you into the type of traversal necessary
- Not creating a helper function
    - A helper function can help you add parameters and retain memory.
## 4: I-mplement

> **Implement** the code to solve the algorithm.

**SOLUTION 1:**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        # Create a helper function to process each node and flag for root node to prevent repeated calls to helper from same node. 
        def helper(root: Optional[TreeNode], targetSum: int, isRoot: bool):
            nonlocal numberOfPaths

            # Set basecase to root is None, return
            if not root:
                return 

            # Upon reaching a targetSum of zero, increase number of paths by 1
            if targetSum - root.val == 0:
                numberOfPaths += 1
            
            # Recursively progress through entire tree from previous node
            helper(root.left, targetSum - root.val, False)
            helper(root.right, targetSum - root.val, False)

            # Recursive progress through entire tree from current node
            if isRoot:
                helper(root.left, targetSum, True)
                helper(root.right, targetSum, True)

        # Create variable to hold number of paths found
        numberOfPaths = 0

        # Call helper function to populate number of paths found
        helper(root, targetSum, True) 

        # Return the number of paths found
        return numberOfPaths
```

**SOLUTION 2:**
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def pathSum(self, root, target):        
        # Create a helper function to process each node and store and release past path sums. Past path sums are used to emulate all possible starting points.
        def helper(root, target, currPathSum):
            nonlocal result

            # Set basecase to root is None, return
            if root is None:
                return 

            # Generate current path sum by adding current node
            currPathSum += root.val

            # Generate past path starting points by subtracting current path sum and target. 
            # This is the value we can start from to get the target. Have we seen it? How many times have we seen it?
            pastPathSum = currPathSum - target
            
            # Add the number of ways we can get to TargetSum from this node, by checking the number of starting points that will get us to target.
            result += cache.get(pastPathSum, 0)

            # Add this path to the cache as part of the pastPathSum
            cache[currPathSum] = cache.get(currPathSum, 0) + 1
            
            # Recursively check left child and right child
            helper(root.left, target, currPathSum)
            helper(root.right, target, currPathSum)

            # Release current path sum upon closing resursive call, because current path sum(Starting Point) will no longer be available.
            cache[currPathSum] -= 1

        # Create variable to hold number of paths found
        result = 0

        # Cache the first possible starting point of zero.
        cache = {0:1}

        # Call the helper function to find the number of paths
        helper(root, target, 0)
        
        # Return the results
        return result
```
```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        // Cache the first possible starting point of zero
        Map<Long, Integer> map = new HashMap<>();
        map.put(0L, 1);

        // Return the helper function to find the number of paths
        return helper(root, (long) targetSum, 0, map);
    }

    // Create a helper function to process each node and store and release past path sums. Past path sums are used to emulate all possible starting points.
    private int helper(TreeNode node, long targetSum, long preSum, Map<Long, Integer> map) {
        // Set basecase to root is None, return
        if (node == null) {
            return 0;
        }
        // Generate current path sum by adding current node
        preSum += node.val;

        // Generate past path starting points by subtracting current path sum and target
        // This is the value we can start from to get the target. Have we seen it? How many times have we seen it?
        int res = map.getOrDefault(preSum - targetSum, 0);

        // Add the number of ways we can get to TargetSum from this node, by checking the number of starting points that will get us to target.
        // Add this path to the cache as part of the pastPathSum
        map.put(preSum, map.getOrDefault(preSum, 0) + 1);

        // Recursively check left child and right child
        res += helper(node.left, targetSum, preSum, map) + helper(node.right, targetSum, preSum, map);
        
        // Release current path sum upon closing resursive call, because current path sum(Starting Point) will no longer be available.
        map.put(preSum, map.get(preSum) - 1);

        // Return the results
        return res;
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number nodes in tree

**SOLUTION 1:**

* **Time Complexity**: `O(N^2)` because we need to visit each node in binary tree and at each node we will need to visit each node in the binary tree. Technically, it's `O(N * N/2)` because we will be reducing the number nodes that needs to be visited at each node as we traverse down the tree.
* **Space Complexity**: `O(1)` if we do not count the recursion stack. The recursion stack will cost us `O(N)` if we have a linked list like tree. `O(logN)` in the average case. If we had a `O(N)` space complexity we could reduce the time complexity down to `O(N)`, by using a hashmap. 
  
**SOLUTION 2:**

* **Time Complexity**: `O(N)` because we only need to visit each node in binary tree once.
* **Space Complexity**: `O(N)`, by using a hashmap to hold the past path sums to be used as starting points. 