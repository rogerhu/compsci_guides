## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/>
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Binary Trees, Binary Search Trees
* 🗒️ **Similar Questions**: [Convert Sorted List to Binary Search Tree](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input array be null or empty?
  - Yep! In that case, let’s just return a null reference.
- Could values be negative?
  - Yes. Our solution shouldn’t change because of that.
- After creating the tree, do we need to verify its height-balanced property?
  - I don’t believe you need to verify it after you make it. Rather, your process in generating the tree should guarantee that it should be height-balanced. Does that make sense?
   
```markdown
HAPPY CASE
Input: [1, 2, 3, 4, 5]
Output:
        3
       / \
      2   4
     /     \
    1       5

Input: [1, 2, 3, 4, 5, 6, 7, 8]
Output:
        4
      /   \
     2     6
    / \   / \
   1   3 5   7
              \
               8
(Note the subtrees differ in height by 1, but not more than 1)

EDGE CASE (One element)
Input: [1]
Output:  
    1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
  - Since we don’t have a tree to start with, we cannot use these traversals to help us in this problem. However, performing an in order traversal of this tree after generating it would yield ascending values.
Using binary search to find an element
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue
We could apply a binary search strategy to this problem to divide the array the way a BST divides a sorted data set.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

Start at the array middle and recurse left, right halves of the array to build a eight-balanced binary search tree.

```markdown
1) Calculate center point of current array
2) Create node of center value
3) Set left reference to center of the left half of array (recurse)
4) Set right reference to center of the right half of array (recurse)
5) Return current node
```

**⚠️ Common Mistakes**

* When we look at a BST, from left to right, the elements are sorted. When we look at an array, from left to right, the elements are sorted. Can we use this shared pattern between the two to help us generate the BST from the array? If we chose a random index in the array, everything left of that index is smaller and everything right of that index is larger, right? If we were to structure our BST on that index it may not be height-balanced, so how do we choose a proper index? Some people don’t see the connection between the sorted array and the sorted nature of a BST. This alignment offers a simple solution to the problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    int[] nums;

    public TreeNode helper(int left, int right) {
        if (left > right) return null;

        // always choose left middle node as a root
        int p = (left + right) / 2;

        // preorder traversal: node -> left -> right
        TreeNode root = new TreeNode(nums[p]);
        root.left = helper(left, p - 1);
        root.right = helper(p + 1, right);
        return root;
    }

    public TreeNode sortedArrayToBST(int[] nums) {
        this.nums = nums;
        return helper(0, nums.length - 1);
    }
}
```
```python
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:        
        def helper(left, right):
            if left > right:
                return None

            # always choose left middle node as a root
            p = (left + right) // 2

            # preorder traversal: node -> left -> right
            root = TreeNode(nums[p])
            root.left = helper(left, p - 1)
            root.right = helper(p + 1, right)
            return root
        
        return helper(0, len(nums) - 1)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(n) since we visit each node exactly once
* Space Complexity: O(logN), since the tree is height-balanced. 