## Introduction
A common problem we are posed with is to search for something in a given list. For example, given the numbers of a lottery draw `1, 2, 5, 10, 16, 20, 23, 36, 40, 41, 45`, we may want to find out if our number `18` is a winning number.

If we didn't know that the list of winning numbers is sorted, we would have to look at every number. However, if we know the list is sorted, we can stop once we reach a number bigger than the one we're looking for.

We can be even more efficient in our search by starting in the middle in the list, and moving left or right depending on whether the number we're looking for is smaller or bigger than the number we're currently comparing to.

For example, if our list is `1, 2, 5, 10, 16, 20, 23, 36, 40, 41, 45` and we were looking for `18`, we would 
- first compare 18 and 20 (because 20 is in the middle of the list)
- Since 18 is less than 20, we would then move to compare 18 and 5 (since 5 is the middle of the list `1, 2, 5, 10, 16`)
- 18 is larger than 5, so we then move to the right side of the remaining list (`10 16`)
- we would keep this process going until we have no more numbers to compare with

The example we saw above is often put in the form of **binary search trees**. 

**Binary Search Trees (BST)** are special cases of binary trees. Binary trees are trees where each element or node has no more than 2 children.

The tree below **is** a binary tree

            1
        2       3
      4   5
      

Binary trees are composed of **nodes**, where each node in the tree has at most 2 children. Nodes typically hold  the following properties:
- data element (usually an integer,  but can be anything)
- left pointer
- right pointer


```c
class TreeNode {
    int data;
    TreeNode left;
    TreeNode right;
}
```
**Language: Python**
```python
class TreeNode():
 def __init__(self, data=None, left=None, right=None):
	self.data = data
	self.left = left
	self.right = right
```

`1` `2` `3` `4` `5` are all nodes in the tree above.

This tree, however, **is not** a binary tree because `1` has 4 children ` 2 3 4 5`

                1
        2    3     4    5

**BSTs are special in that for each node, all nodes to the left are less than or equal to it, and all nodes to the right are greater than it**. Due to this property, **lookups in a BST take O(logN) time on average** if the tree is well-balanced.

If we were to put our winning lottery numbers into BST form, it could look something like this:
`1, 2, 5, 10, 16, 20, 23, 36, 40, 41, 45`

                        20
                5                40
            2       16        36     41
        1         10        23           45

## Pros and Cons
BSTs are the best options when trying to optimize for **efficient searching and flexible updates**. Unsorted linked lists have O(1) insertion and deletion operations, but require O(N) for search operations. If an array is sorted, searching can be O(logN), but updating the array by adding or deleting elements is can be O(N) in the worst cases.

On the other hand, BSTs have more overhead and complexity to initialize and maintain. Arrays and linked lists are more straight forward due to their one dimensional structure. Trees require more thought due to its multi-dimension structure.

## Time and Space Complexity
**Best cases:**
Accessing / Searching : `O(logN)`
Inserting: `O(logN)`
Deleting: `O(logN)`

**Worst cases:**
Accessing / Searching : `O(n)`
Inserting: `O(n)`
Deleting: `O(n)`

The best / worst cases differ by how well the tree is balanced. Balanced trees are more efficient than unbalanced trees.

Balanced tree:

        (5)
    (3)     (7)
    
Unbalanced tree:

            (5)
        (3)
    (1)  

## Glossary

 * **Leaf** nodes have no left or right children.
 * **Full binary tree** means every node has exactly zero or two children nodes. 
 * **Balanced binary tree** means the depth difference between two leaves is at most 1.
 * The **order** of a node (or tree) is its number of children.
 * **Height** of a node is the number of edges on the longest path from the node to a leaf. 

## Traversing Binary Trees

There are 4 main methods for traversing binary trees: preorder, postorder, inorder, or level order (breadth first search). Most tree problems can be solved with one of these methods. The challenging part is figuring out which traversal to use. 

![](https://i.imgur.com/t2Ihbru.png)

**Preorder**




```c
void printPreorder(TreeNode node) {	
    if (node == null) {
        return;
    }
    System.out.print(node.data + " "); // process node
    printPreorder(node.left); // recurse on left
    printPreorder(node.right); // recurse on right
}
```

**Language: Python**
```python
def printPreorder(node:TreeNode):	
    if (node == None):
        return
    print(node.data)
    printPreorder(node.left)
    printPreorder(node.right)
```

Output: 1 -> 2 -> 4 -> 5 -> 3
Good for exploring roots before leaves. 
Example problems: copying a tree

**Postorder**


```c
void printPostorder(TreeNode node) {
    if (node == null) {
        return;
    }
    printPostorder(node.left); // recurse on left
    printPostorder(node.right); // recurse on right
    System.out.println(node.data + " "); // process node
}
```
**Langauge: Python**

```python
def printPostorder(node:TreeNode):
    if (node == None):
        return
    printPostorder(node.left)
    printPostorder(node.right)
    print(node.data)
```

Output: 4 -> 5 -> 2 -> 3 -> 1
Good for exploring leaves before roots.

**Inorder**


```c
void printInorder(TreeNode node) {
    if (node == null) {
        return;
    }
    printInorder(node.left); // process left
    System.out.println(node.value); // process node
    printInorder(node.right); // process right
}
```
**Langauge: Python**
```python
def printInorder(node:TreeNode):
    if (node == None):
        return
    printInorder(node.left)
    print(node.data)
    printInorder(node.right)
```

Output: 4 -> 2 -> 5 -> 1 -> 3
Good for converting BST into an array.

**BFS**


```c
public void printBFS(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<BinaryTree.TreeNode>();
    
    if (root == null)
        return;
    queue.clear();
    queue.add(root);
    while(!queue.isEmpty()){
        TreeNode node = queue.remove();
        System.out.print(node.element + " ");
        if(node.left != null) queue.add(node.left);
        if(node.right != null) queue.add(node.right);
    }

}
```
**Language: Python**

```python
from collections import deque
def printBFS(root:TreeNode):
    queue = deque()
    if (root == None):
        return
    queue.append(root)
    while queue:
        node = queue.popleft()
        print(node.data)
        if(node.left != None): queue.append(node.left)
        if(node.right != None): queue.append(node.right)
```
Output: 1 -> 2 -> 3 -> 4 -> 5

## Common Operations

### Searching in a BST



```c
public boolean doesNodeExistInBST(TreeNode bstRoot, int searchValue) {
    // if we've ran out of values to search for, return false
    if (bstRoot == null) {
        return false;
    } else if (bstRoot.value == searchValue) {
        return true;
    } else {
        // if the node we're at is smaller than the value we're looking for, traverse on the right side
        if (searchValue > bstRoot.value) {
            return doesNodeExistInBST(bstRoot.right, searchValue);
        } else {
            // if the node we're at is bigger than the value we're looking for, traverse the left side
            return doesNodeExistInBST(bstRoot.left, searchValue);
        }
    }
}
```
**Language: Python**

```python
def doesNodeExistInBST(bstRoot:TreeNode, searchValue:int):
    # if we've ran out of values to search for, return false
    if (bstRoot == None):
        return False
    elif (bstRoot.data == searchValue):
        return True
    else:
        #if the node we're at is smaller than the value we're looking for, traverse on the right side
        if (searchValue > bstRoot.data):
            return doesNodeExistInBST(bstRoot.right, searchValue)
        else:
            #if the node we're at is bigger than the value we're looking for, traverse the left side
            return doesNodeExistInBST(bstRoot.left, searchValue)
```
**Common related problems:**
Sum of left leaves (Easy)
https://leetcode.com/problems/sum-of-left-leaves

Find root to leaf path with specified sum (Easy)
https://www.geeksforgeeks.org/root-to-leaf-path-sum-equal-to-a-given-number/

Sum of root to leaf numbers (Medium)
https://leetcode.com/problems/sum-root-to-leaf-numbers/description/

Find leaves (Medium)
https://www.programcreek.com/2014/07/leetcode-find-leaves-of-binary-tree-java/

### Height of Binary Tree
The height of a tree is the length of the path from the root to the deepest node in the tree.



```c
int getBinaryTreeHeight(TreeNode node) {
    if (node == null) {
        return -1;
    }

    int leftHeight = getBinaryTreeHeight(node.left);
    int rightHeight = getBinaryTreeHeight(node.right);

    if (leftHeight > rightHeight) {
        return leftHeight + 1;
    } else {
        return rightHeight + 1;
    }
}
```
**Langauge: Python**
```python
def getBinaryTreeHeight(node:TreeNode):
    if (node == None):
        return -1
    leftHeight = getBinaryTreeHeight(node.left)
    rightHeight = getBinaryTreeHeight(node.right)

    if (leftHeight > rightHeight):
        return leftHeight + 1
    else:
        return rightHeight + 1
```

### Depth of Binary Tree
What's the difference between the height and depth of a binary tree?

https://stackoverflow.com/questions/2603692/what-is-the-difference-between-tree-depth-and-height

## Patterns List
* [Binary Trees Iterative Traversal Approach](https://guides.codepath.com/compsci/Binary-Trees-Iterative-Traversal)
* [Finding 2nd Largest Node in a Binary Search Tree](https://guides.codepath.com/compsci/Binary-Trees-2nd-Largest-Node)

## References
### Further reading links
* https://algs4.cs.princeton.edu/32bst/
* http://cslibrary.stanford.edu/110/BinaryTrees.html
* https://inst.eecs.berkeley.edu/~cs61b/fa17/materials/lectures/lect20.pdf
* [Different Types of Binary Tree with colourful illustrations](https://towardsdatascience.com/5-types-of-binary-tree-with-cool-illustrations-9b335c430254)

### Videos
* [Gayle Laakmann McDowell's Overview of Trees](https://www.youtube.com/watch?v=oSWTXtMglKE)
* [Walkthrough of Inverting Binary Tree](https://www.youtube.com/watch?v=2zLM6K1RItQ)
* [Walkthrough of Validating Binary Search Tree](https://www.youtube.com/watch?v=PXyaZFOklwU)