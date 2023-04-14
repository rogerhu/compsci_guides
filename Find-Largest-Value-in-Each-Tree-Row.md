## Problem Highlights

* 🔗 **Leetcode Link:** [Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Trees
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input tree be null?
  - Yes. In that case, let’s return an empty list. That makes the most sense.
- Could there be a tie for the largest value in a row?
  - Yes, there can be. If there is a tie, we just choose that value.
   
```markdown
HAPPY CASE
Input: 
          1
         / \
        3   2
       / \   \  
      5   3   9 

Output: [1, 3, 9]

Input: 
         -1
         / \
        3   2
       /   /    
      5   2     

Output: [-1, 3, 5]

EDGE CASE
Input: 
Output: []

Input: 0
Output: [0]

Input: 1
Output: [1]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:

- Using a Pre/In/Post-Order Traversal to generate a unique sequence of nodes
We want to be able to compute maximum values a row at a time. These types of traversals don’t perform level-order traversals that are needed in this problem.
- Using Binary Search to find an element
We aren’t looking for a specific value, so its hard to use Binary Search in this problem. In addition, the tree is not a BST, so Binary Search couldn’t apply to this problem.
- Applying a level-order traversal with a queue
Using a level-order traversal for this problem is essential since we are looking for a computation on each level.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

Perform a level-order traversal of the tree with a Queue. Calculate a running max while traversing the row.

```markdown
1) Create a Queue and place the root in there
2) While the Queue is not empty, iterate through the queue
    a) Compute the length of the Queue and iterate that fixed amount (k) 
        for the current row
        i) Keep a running max for the row
        ii) Add the left and right sub-tree nodes to the end of the queue for the 
            next iteration
    b) Append this row's max to an output array
3) Return the array of row-wise maximums
```

**⚠️ Common Mistakes**

* Your interviewee may not know about level-order traversals in trees. As an effect, they may try to traverse the tree with a pre/in/post order traversal and store level associations in a HashMap.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        search(root, 0, result);
        return result;
    }

    public static void search(TreeNode root, int depth, List<Integer> result) {
        if (root == null) {
            return;
        }
        if (depth >= result.size()) { // if this row of the tree has not been visited yet
            result.add(root.val); // add the value
        } else { // if this row has been visited already
            int cur = result.get(depth); // get the previous max value of the row
            int big = Math.max(cur, root.val); // compare
            if (cur != big) { // if the node we are visiting now has the max value
                result.remove(depth); // remove the previous
                result.add(depth, big); // add the max
            }
            
        }
        search(root.left, depth+1, result); // search left and right (the order does not matter)
        search(root.right, depth+1, result);
    }
}
```
```python
import sys, queue

def largestValues(self, root: TreeNode) -> List[int]:
    if not root:
        return []
    output = []
    q = queue.Queue()
    q.put(root)
    while q.qsize() > 0:
        cur_max = -sys.maxsize # running max value for this row
        for _ in range(q.qsize()): # fixed size iteration for this row
            node = q.get()
            cur_max = max(cur_max, node.val)
            if node.left:
                q.put(node.left)
            if node.right:
                q.put(node.right)
        output.append(cur_max)
    return output
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(N), since we had to process each node in the tree
* Space Complexity: O(N), as we used a queue data structure