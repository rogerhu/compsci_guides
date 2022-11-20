## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/find-duplicate-subtrees/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Binary Trees
* 🗒️ **Similar Questions**: [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/), [Construct String from Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What will I be given as input?
  - You will be given the root of a binary tree as input
- What should I output?
  - You should output the roots of subtrees that appear 2+ times in the input tree. For each subtree structure that appears multiple times, you should output the root node of one of them.
- How do we determine if one subtree is equal to another?
- Is there a way to uniquely identify a subtree's structure?
- Try traversing the tree recursively and store each subtree as a combination of the root's value, its left subtree, and right subtree.

   
```markdown
HAPPY CASE
Input:              [1]
               /            \
             [2]            [5]
          /      \              \
        [3]      [4]            [6]
                               /    \
                             [5]    [2]
                                  /      \ 
                                [3]      [4]   
Output:     [2]             and all its subtrees ([3] and [4]) are duplicate subtrees found in the input tree, so we should return pointers to one of the [2] nodes, one of the [3] nodes, and 
          /      \          one of the [4] nodes.
        [3]      [4] 

EDGE CASE
Input: NULL  
Output: Since there are no nodes passed in, we should return NULL
(this would be a good clarifying question to ask your interviewer).
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a traversal (ie. Pre-Order, In-Order, Post-Order, Level-Order)
  - With a traversal we can visit every node, including the leaf nodes
  - If we can find the depth of every leaf, we can find the min depth
  - We should find a way to keep track of the depth of each node we visit while traversing
  - We can visit each node one by one, and if its value matches the value of a previously seen node, we can try comparing their subtrees to see if their structures are equal. We would compare them by traversing the trees simultaneously, and on each node visit we should see 2 nodes with the same value. This would also require using some data structure to store previously seen nodes so we can compare them to new nodes we visit
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue
- Utilizing a dictionary: We can create a mapping from a node to an ID that describes the subtree rooted at the node
For a node n, its id will be (n.val, ID(n.left), ID(n.right)) (ID is NULL if the node is NULL). We can visit the nodes in the tree, and set the IDs of every node. Then, we can iterate through the tree an additional time, and if we come across a node with an ID we have seen before, we know that there is a matching subtree rooted at that node


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
Setup: create a dictionary that maps node values to nodes to ids
1) Traverse the tree
2) When we visit a node, generate its ID to be (node.val, ID(node.left), ID(node.right))
    - the ids of the left and right children are computed recursively
    - Save the mapping of a node to its ID in a dictionary
3) Traverse the tree again, storing IDs as we go
    - When we come across a node whose ID we have already stored, add the corresponding node to the result array
        - Be careful here to make sure we don't add a node that is a duplicate match (we can track a set of IDs 
            that correspond to nodes that have already been added, and only add a node if its ID is not in this set)
```

**⚠️ Common Mistakes**

* The time complexity is a little tricky to determine, because technically it depends on how the IDs are passed up the call stack and compared:
- An ID is essentially a tuple of three values: the node's value, the left child's ID, and the right child's ID
- However, since the children's ID are used to define the parent node's ID, this tuple could contain something like this:
    - ID = (node.val, (node.left.val, (node.left.left.val, ..., ...), (node.left.right.val, ..., ...)), (node.right.val, (node.right.left.val, ..., ...), (node.right.right.val, ..., ...)))
    - Essentially, we will have a bunch of nested IDs inside IDs, so the effective length of ID would be larger than just 3 values, rather it would be O(N)
    - If these tuples are stored in memory and passed/compared by reference (or hashing), then ID generation and comparison would take O(1) time per ID, resulting in O(N) time to generate and compare all IDs
    - If the tuples are generated/compared by generating/comparing each value individually (including all the nested values), then ID generation and comparison takes O(N) time per ID, resulting in O(N^2) time

* Be careful not to put duplicate subtree matches in the result array

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    Set<TreeNode> set;
    Map<String, TreeNode> map;
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        set = new HashSet<>();
        map = new HashMap<>();
        traverse(root);
        List<TreeNode> ans = new LinkedList<>();
        for (TreeNode node : set) {
            ans.add(node);
        }
        return ans;
    }
    
    public String traverse(TreeNode root) {
        if (root == null) {
            return "";
        }
        String left = traverse(root.left);
        String right = traverse(root.right);
        String cur = left +" "+ right +" "+ root.val;
        if (map.containsKey(cur)) {
            set.add(map.get(cur));
        } else {
            map.put(cur, root);
        }
        
        return cur;
    }
}
```
```python
 def findDuplicateSubtrees(root: TreeNode) -> List[TreeNode]:
        def createIDs(node, ids):
            if not node:
                return None
            
            # A node's id will be a tuple of its value, the ID of its left child, and the id of its right child
            newId = (node.val, createIDs(node.left, ids), createIDs(node.right, ids))
            
            # store ID in the dictionary
            ids[node] = newId
            
            # return the ID so the parent node can access it
            return newId

        # Create dictionary of ids
        ids = {}
        createIDs(root, ids)
        
        # result set will store the answer
        result = set()

        # seenIDs will store IDs we have seen to detect duplicates
        seenIDs = set()

        # addedIDs will store IDs of nodes in result, so we can ensure we don't add duplicates
        addedIDs = set()
        for node, id in ids.items():
            # if an id has already been seen and a node with the same ID has not already been added to the result
            # then we can add to the result
            if id in seenIDs and id not in addedIDs:
                result.add(node)
                addedIDs.add(id)
            else:
                # otherwise, we only update the ID as being seen
                seenIDs.add(id)
        # return result as a list
        return list(result)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(N^2), since string copying takes O(n) extra time.
* Space Complexity: O(N^2), since we are hashing a string for each node and length of this string can be of the order N.