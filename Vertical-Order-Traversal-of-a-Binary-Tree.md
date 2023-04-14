## Problem Highlights

* 🔗 **Geeks for Geeks link:** [Print Binary Tree Vertical Order](https://www.geeksforgeeks.org/print-binary-tree-vertical-order/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Binary Trees
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input tree be null?
  - Yep! In that case, let’s return an empty list
- Is the input tree a Binary Search Tree?
  - No, the input tree is a binary tree. There are no guarantees about the order of the tree's nodes.
- How can we keep track of our horizontal distance to the center root while traversing the tree?
- What data structure could we use for this problem?
- Is there a way we can store the nodes at a certain horizontal distance from the center?

```markdown
HAPPY CASE

Input:    1
         / \
        3   2
       / \   \  
      5   3   9 

Output: [[5], [3], [1, 3], [2], [9]]

Input:    3
         / \
        9   20
           /  \  
          15   7

Output: [[9], [3, 15], [20], [7]]

 
EDGE CASE
Input:    1
        /   \
       2     3
      / \    / \  
     4   5  6   7

Output: [[4],[2],[1,5,6],[3],[7]] or [[4],[2],[1,6,5],[3],[7]]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a Pre/In/Post-Order Traversal to generate a unique sequence of nodes
The type of traversal does not matter in this case, since all traversals we know of don’t follow a vertical order style. Instead, the student should choose a traversal they are comfortable with to traverse the tree since we need all nodes stored at some point.

- Using Binary Search to find an element
Since the input tree is not a BST, we cannot apply binary search techniques to this problem.

- Applying a level-order traversal with a queue
This problem is very similar to a level-order traversal. However, we are performing the opposite type of traversal. Having the knowledge of performing a level-order traversal can definitely help solve this problem.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

Traverse through the tree and keep track of the horizontal distance to the center as we traverse. Place node values in a HashMap with the key being their horizontal distance from the center.

```markdown
1) Create a HashMap to store hd -> nodes [Int --> List]
2) Keep track of min hd and max hd with an array
3) Traverse through the tree using pre-order traversal (others work as well)
    a) Keep track of current horizontal distance to root
        i) Update min/max hd array
    b) Place node value in List with the proper 'hd' key
    c) Call left with 'hd - 1'
    d) Call right with 'hd + 1'
4) Iterate from min_hd -> max_hd and trasfer data into a nested list
5) Return nested list
```

**⚠️ Common Mistakes**

* If the question was just to find nodes in the same column (where nodes in a column could be from top to bottom OR bottom to top or random): DFS would be enough. But that is not really a vertical order traversal. You can mistakenly use DFS first to find out later in my answer the nodes in the same column were not arranged from top to bottom.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Triplet<F, S, T> {
    public final F first;
    public final S second;
    public final T third;

    public Triplet(F first, S second, T third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    List<Triplet<Integer, Integer, Integer>> nodeList = new ArrayList<>();

    private void DFS(TreeNode node, Integer row, Integer column) {
        if (node == null)
            return;
        nodeList.add(new Triplet(column, row, node.val));
        // preorder DFS traversal
        this.DFS(node.left, row + 1, column - 1);
        this.DFS(node.right, row + 1, column + 1);
    }

    public List<List<Integer>> verticalTraversal(TreeNode root) {
        List<List<Integer>> output = new ArrayList();
        if (root == null) {
            return output;
        }

        // step 1). DFS traversal
        DFS(root, 0, 0);

        // step 2). sort the list by <column, row, value>
        Collections.sort(this.nodeList, new Comparator<Triplet<Integer, Integer, Integer>>() {
            @Override
            public int compare(Triplet<Integer, Integer, Integer> t1,
                    Triplet<Integer, Integer, Integer> t2) {
                if (t1.first.equals(t2.first))
                    if (t1.second.equals(t2.second))
                        return t1.third - t2.third;
                    else
                        return t1.second - t2.second;
                else
                    return t1.first - t2.first;
            }
        });

        // step 3). extract the values, grouped by the column index.
        List<Integer> currColumn = new ArrayList();
        Integer currColumnIndex = this.nodeList.get(0).first;

        for (Triplet<Integer, Integer, Integer> triplet : this.nodeList) {
            Integer column = triplet.first, value = triplet.third;
            if (column == currColumnIndex) {
                currColumn.add(value);
            } else {
                output.add(currColumn);
                currColumnIndex = column;
                currColumn = new ArrayList();
                currColumn.add(value);
            }
        }
        output.add(currColumn);

        return output;
    }
}
```
```python
class Solution:
    def verticalTraversal(self, root: TreeNode) -> List[List[int]]:
        node_list = []

        def DFS(node, row, column):
            if node is not None:
                node_list.append((column, row, node.val))
                # preorder DFS
                DFS(node.left, row + 1, column - 1)
                DFS(node.right, row + 1, column + 1)

        # step 1). construct the node list, with the coordinates
        DFS(root, 0, 0)

        # step 2). sort the node list globally, according to the coordinates
        node_list.sort()

        # step 3). retrieve the sorted results grouped by the column index
        ret = []
        curr_column_index = node_list[0][0]
        curr_column = []
        for column, row, value in node_list:
            if column == curr_column_index:
                curr_column.append(value)
            else:
                # end of a column, and start the next column
                ret.append(curr_column)
                curr_column_index = column
                curr_column = [value]
        # add the last column
        ret.append(curr_column)

        return ret
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Let N be the number of nodes in the input tree.

* Time Complexity: O(NlogN) since we used DFS and we sorted the obtained list of coordinates which contains N elements.
* Space Complexity: O(N). we used a queue data structure to maintain the order of visits.