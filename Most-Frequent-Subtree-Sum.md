## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/most-frequent-subtree-sum/>
* 💡 **Difficulty:** 
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Trees
* 🗒️ **Similar Questions**: [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What exactly should I output?
  - You should output a list of the most frequent subtree sums. If the subtree sum that appears the most times is unique, then a list with only one element should be outputted. If there are multiple subtree sums that appear most frequently, you should output a list containing all of those sums.
- Can the input be a null pointer?
  - No, all inputs will be trees with at least one node
- For a given node n, what components makeup n's subtree sum?
- How does the subtree sum of the subtree rooted at a node n relate to the sum of the subtrees rooted at the direct children of n?
- After working through some examples, you should see that a subtree's sum is the value of its root node plus the subtree sums of the root's direct children. How can we use this paradigm to structure our solution?
   
```markdown
HAPPY CASE
Input: root = [5,2,-3]
Output: [2,-3,4]

Input:              [1]
               /            \
             [2]            [5]
          /      \              \
        [3]      [4]            [6]   
Output: The subtrees of this input are [3], [4], [6],      [2]      , [5]    , and the entire tree.
                                                         /     \        \
                                                       [3]     [4]      [6]
The sums of each of these subtrees are [3, 4, 6, 9, 11, 12] respectively. Each of these sums appears only once, so they would be tied for the most frequent subtree sum.
Therefore, we should return [3, 4, 6, 9, 11, 12] as the answer.

EDGE CASE
Input: [3]  
Output: There is only one subtree (the input tree itself) and its sum is 3, so we should return [3] 
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


For trees, some things we should consider are:
- Using a traversal
  - We can visit each node one by one, and calculate the sum of the subtree recursively (by visiting its children)
An important note here is that the sum of a node's value, the node's left subtree sum, and the node's right subtree sum will equal the subtree sum of the tree rooted at that node. So, if we can return the sum of the child subtree as we recursively visit nodes, we can calculate a node's subtree sum by using previously computed subtree sums.
- Utilizing a dictionary
  - In order to track the frequency of a subtree sum, we need to use a data structure to hold frequencies of sums
A hashmap with key = the subtree sum and val = the frequency of that subtree sum as we compute will work nicely
After populating this hashmap, we need to find the most frequent sums and return a list with only those sums
- Using binary search to find an element
  - The tree is not ordered in any way, and we should probably visit all leaves, so searching for a specific node won't work
- Storing nodes within a HashMap to refer to later
- Applying a level-order traversal with a queue


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

Traverse the tree with a post order traversal. Get the subtree sum by adding the node's value to the left subtree sum + right subtree sum (which are calculated recursively), and use a hashmap to store frequencies of sums. Then iterate over the hashmap to return those sums which appear most frequently.

```markdown
Setup: create a dictionary that maps node values to nodes to ids
1) Create a hashmap to store frequencies of sums
2) Traverse the tree
3) When we visit a node, generate its subtree sum
    - This will equal the value of its left subtree sum + value of its right subtree sum + the node's value
    - The left/right subtree sums can be computed recursively as we visit
4) Once we generate the subtree's sum s, increment frequency of s by 1 in the hashmap (or set frequency to 1 if this is first time adding that key to map)
5) Return the computed subtree sum so that the caller (the node's parent) can use it to calculate its own subtree sum
6) After traversal is complete, iterate over the hashmap, tracking the highest frequency sum and the frequency at which it appears
    - Keep a list that contains the sums with highest frequency so far, and a variable tracking the highest frequency encountered
    - If we encounter a key (a subtree sum) whose value (the frequency of that sum) exceeds the highest frequency encountered, we reset the list to only contain that key, and update the variable tracking highest frequency encountered
    - If we encounter a key (a subtree sum) whose value (the frequency of that sum) equals the highest frequency encountered, then add that key to the list
    - At the end of this iteration, we will have the list of subtree sums with highest frequency
```

**⚠️ Common Mistakes**

* Be careful when doing the traversal to consider special cases for nodes (if the node is a leaf, or if only one of its children is defined)

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```java
class Solution {
    int max = -1; // maximum frequency of any the subtree sum
    public int[] findFrequentTreeSum(TreeNode root) {
        Map<Integer,Integer> map = new HashMap<>(); // to keep track of frequency of each subtree sum
        traverse(root,map); 
        List<Integer> res = new ArrayList<>(); // result list
        for(int i:map.keySet()) if(map.get(i) == max) res.add(i); // adding the subtree sum values that have max frequency
        return res.stream().mapToInt(i->i).toArray();  // convert list to array the return 
    }
    public void traverse(TreeNode root,Map<Integer,Integer> map){
        if(root == null) return;
        traverse(root.left,map);
        traverse(root.right,map); 
        // bottom-up
        int sum=root.val; // after reaching the leaf node is is obvious that the subtree sum will be node's value itself
        if(root.left != null) sum+= root.left.val; // if not the leaf node we will calculate the subtree sum
        if(root.right != null) sum+= root.right.val;
        map.put(sum,map.getOrDefault(sum,0)+1); // tracking the frequency of the sunstree sum
        max = Math.max(max,map.get(sum)); // keeping track of the maximum frequency of any subtree sum
        root.val = sum; // as we are operating bottom-up we will update the value of the root with it's subtree sum
    }
}
```
```python
from collections import defaultdict
class Solution:
    def findFrequentTreeSum(self, root: Optional[TreeNode]) -> List[int]:
        # initialize a hashmap that counts frequencies of a sum
            # Note: a defaultdict in python allows for updating the value of key that does not exist in the dict
            # For example, if we try to increment the value of a key k by 1 but k is not in the defaultdict, it will
            # initialize a new entry with key = k and value = 0 (the default value for a defaultdict of ints) automatically,
            # and then perform the increment step
        frequency_of_sums = defaultdict(int)
        def visit(node):
            # if we are at a null node, its sum will be 0
            if not node:
                return 0
            
            # the subtree sum of the subtree rooted at the current node is equal to the subtree sum of the left child's
            # subtree + subtree sum of the right child's subtree + the node's value
            subtree_sum = visit(node.left) + visit(node.right) + node.val

            # increment the frequency of this subtree sum by 1 in the hashmap
            frequency_of_sums[subtree_sum] += 1

            # return the computed subtree sum up the call stack so the parent who visited this node can calculate its subtree sum
            return subtree_sum
        
        # perform tree traversal
        visit(root)

        # find the most frequent subtree sum(s)

        # initialize an array that will hold the most frequent subtree sums
        most_frequent = []

        # initialize the highest frequency seen so far to be 0
        highest_frequency = 0
        for s in frequency_of_sums:
            # if there are no sums processed yet, just initialize the most frequent sum to be the first sum processed
            if len(most_frequent) == 0:
                most_frequent.append(s)
                highest_frequency = frequency_of_sums[s]
            # if we come across a sum whose frequency is equal to the highest frequency encountered so far, add it to the result array
            elif frequency_of_sums[s] == highest_frequency:
                most_frequent.append(s)
            # if we come across a sum whose frequency exceeds the highest frequency encountered so far, discard the sums we have been tracking
                # and reinitialize to only track this new sum, and update the highest frequency encountered accordingly
            elif frequency_of_sums[s] > highest_frequency:
                most_frequent = [s]
                highest_frequency = frequency_of_sums[s]
        
        # return the list of most frequent sums
        return most_frequent
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of nodes in the tree.

* Time Complexity: O(N)
* Space Complexity: O(N)