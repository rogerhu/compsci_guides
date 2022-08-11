Traversing a binary tree recursively is usually the first approach to approaching binary tree problems. However, recursion could lead to large memory footprints, and often times interviewers will ask for an iterative traversal.

When traversing a tree iteratively, it is common to use a **stack** or a **queue**. 
The common pattern involves:

1) Determine whether to use a stack or a queue to store nodes we need to visit.

    a) stacks are last-in-first-out.
 
    b) queues are first-in-first-out.

2) While our stack/queue is not null, retrieve nodes from it.

    a) When we pop a node to visit, we also have to figure out how to push its child nodes.

As an example, we can take a look at how to implement a preorder traversal iteratively.

Recall the **recursive approach** for a preorder traversal:

**Language: C**

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

```
def printPreorder(node:TreeNode):
    if node == None:
        return
    print(node.data)
    printPreorder(node.left)
    printPreorder(node.right)
```

For the following tree:

![](https://i.imgur.com/t2Ihbru.png)

Our preorder traversal would be:
`1 -> 2 -> 4 -> 5 -> 3`

At this point, we know a couple of things
- We want to visit roots before leaves
- We want to visit the left child before the right child.

1) Let's say, we visit each node with a stack. The first node we visit will always be the root. So in the example above, `1` is the root. 

2) When we pop `1` from the stack, we have an option to add node `2` first or node `3` first. Which node should we push onto the stack first?

   * Looking at what our traversal should end up being, `2` comes before `3`, so if we want to see `2` first, we should probably add node `3` to the stack, followed by node `2`. That way, when we pop a node from the stack, `2` will be popped before `3`

At this point, we've printed `1` and our stack looks like this:

    (2)
    (3)

3) As our stack is not empty yet, we can pop from it again. We pop`2`, and we have to decide whether to push node `4` first or node `5`. 

   * Again, if we look at our desired traversal outcome from our recursive approach, we see that `4` should be printed before `5`. Following step 2a, it looks like we should push `5` onto the stack first, followed by `4`.

   * Now we've printed `1 2` and our stack looks like this:

     (4)
     (5)
     (3)
    
4) Again, we pop from our stack. This time `4` is popped and printed, and since `4` has no children, we don't push anything, and just keep popping.

   * After `4` has been popped, the function printed `1 2 4` and our stack would then contain:

     (5)
     (3)

Looking at what we've been doing, it looks like a pattern has emerged.
1.  Create an empty stack 
2.  Push root node onto stack
2.  While our stack is not empty:
    a. pop node from the stack and print it
    b. push right child of popped node to stack.
    c. push left child of popped node to stack

```c
public void preorderTraversal(TreeNode root) {
    TreeNode node = root;
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    while (!stack.isEmpty()) {
        TreeNode curr = stack.pop();
        System.out.print(curr.data);
        if (curr.right != null) {
            stack.push(curr.right);
        }
        if (curr.left != null) {
            stack.push(curr.left);
        }
    }
}
```

Time complexity is O(n) since we push/pop each node of the tree, while space complexity is O(h), h being the height of the tree.

Going back to the beginning to see how we approached this problem from start to finish, there are a couple of important steps we followed:
1. Recall the recursive way to solve the problem
2. Working from the recursive solution, we know what our desired output should be
3. Using the desired output, we then tried to create a new approach that would give us the same outcome
4. Once we saw a pattern, we were able come up with an algorithm

## Example problems 

These are problems that can be solved with similar approaches of using stacks / queues

- Binary Tree Iterator:
https://leetcode.com/problems/binary-search-tree-iterator/description/
- Binary Tree Inorder Traversal:
https://leetcode.com/problems/binary-tree-inorder-traversal/
- Binary Tree Postorder Traversal:
https://leetcode.com/problems/binary-tree-postorder-traversal/

## More resources

* Article : https://leetcode.com/articles/binary-tree-inorder-traversal/
* Video: https://www.youtube.com/watch?v=VsxLHGUqAKs
