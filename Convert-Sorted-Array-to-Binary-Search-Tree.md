## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/>
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
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
  - Yes. the node values will range from -100000 to 100000. Our solution shouldn’t change because of that.
- After creating the tree, do we need to verify its height-balanced property?
  - I don’t believe you need to verify it after you make it. Rather, your process in generating the tree should guarantee that it should be height-balanced. Does that make sense?
- Will the input list always be sorted?
  - Yes, the values in the input list will always be sorted in a strictly increasing order
   
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

EDGE CASE (No elements)
Input: []
Output: None
    
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - Since are traversing a array. Tree Traversal does not apply here
- Using binary search to find an element
  - We are not looking to find an element in this problem so this technique is not useful for this problem
- Storing nodes within a HashMap to refer to later
  - We could employ this technique, but is it really necessary?
- Applying a level-order traversal with a queue
  - Using this approach may complicate our code
- We could apply fundamental ideas about sorted arrays and binary search to this problem. 
  - Let's recursively get the mid point of the array as root and recursively call for left child with first half of array and right child with second half of array, because the mid point of each recursive array is the point where we can branch off smaller elements on left and larger elements on right. 
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use fact that the array is sorted and the idea of searching the mid point with each loop during binary search. We recursively get the mid point of the array as root and recursively call for left child with first half of array and right child with second half of array. Because the mid point of each recursive array is the point where we can branch off smaller elements on left and larger elements on right. 

```markdown
1) Basecase: Check for an empty input list. If it is empty, return None (an empty BST)
2) Recursively: 
    a) Get the index of the root node from the list (this will be the element at the center of the input list)
    b) Build the left and right subtrees of the tree by recursively calling the function on each remaining half of the input list
3) Return the root of the tree
```

**⚠️ Common Mistakes**
- Not noticing or clarifying that the input list is sorted
    - Not recognizing how this sorted property allows us to build the BST
    - When we look at a BST, from left to right, the elements are sorted. When we look at an array, from left to right, the elements are sorted. Can we use this shared pattern between the two to help us generate the BST from the array? If we chose a random index in the array, everything left of that index is smaller and everything right of that index is larger, right? If we were to structure our BST on that index it may not be height-balanced, so how do we choose a proper index? Some people don’t see the connection between the sorted array and the sorted nature of a BST. This alignment offers a simple solution to the problem.

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
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        # Basecase: Check for an empty input list. If it is empty, return None (an empty BST)
        if not nums:
            return None

        # Recursively: Get the index of the root node from the list (this will be the element at the center of the input list)
        mid = len(nums) // 2
        root = TreeNode(nums[mid])

        # Recursively: Build the left and right subtrees of the tree by recursively calling the function on each remaining half of the input list
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid + 1:])

        # Return the root of the tree
        return root
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* **Time Complexity**: O(N)
    *  We need to visit each value in the array to create a Binary Search Tree with each value.
* **Space Complexity**: O(N) 
    * O(N) because we need O(N) space to represent each value in the array as nodes in the binary search tree. 