## Problem Highlights

* 🔗 **Leetcode Link:** [Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Graph 
* 🗒️ **Similar Questions**: [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer), [Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Does the graph have to a connected graph?
  - Yes, this graph does not need to be a connected graph. There would be no town judge if graph is disconnected.
- How many people could there be in town?
    - Between 1 and 1000 people
- Are all trust pairs unique, there are no duplicate pairs correct?
    - Yes all trust pairs are unique and there are no duplicate pairs
- What is the time and space constraints for this problem?
    - Let's go with `O(V + E)` time and `O(V)` space. 

```markdown
HAPPY CASE
Input: n = 2, trust = [[1,2]]
Output: 2

Input: n = 3, trust = [[1,3],[2,3]]
Output: 3

EDGE CASE
Input: n = 1, trust = []
Output: 1
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:

- DFS/BFS: This may not a connected graph, a DFS/BFS approach will be less optimal in identifying the town judge.
- Adjacency List: We already have an adjacency list known as trust that we can use.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but this will make the problem more complicated
- Topological Sort: In order to have a topological sorting, the graph must not contain any cycles. We cannot apply this sort to this problem because we can have cycles in our graph.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Since all trust pairs are unique, we can ensure that any person with n-1 trust is the town judge, by reducing trust for the truster and increase trust for the trustee. This way a person who trust another person cannot achieve n-1 trust. 

```markdown
1. Create a list of of person and their trust value
2. Increase trust value of trustee and decrease trust value of truster for each pair
3. Check if anyone achieves n-1 trust. This person is the town judge
4. If no one achieves n-1 trust, then there is no town judge. 
```

⚠️ **Common Mistakes**

* Generating a union set here, would work. However, this will look like a brute force approach to this problem. 
 
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        # Create a list of of person and their trust value
        numTrust = [0] * (n + 1)

        # Increase trust value of trustee and decrease trust value of truster for each pair
        for person1, person2 in trust:
            numTrust[person1] -= 1
            numTrust[person2] += 1
        
        # Check if anyone achieves n-1 trust. This person is the town judge
        for i in range(1, len(numTrust)):
            if numTrust[i] == n - 1:
                return i
        
        # If no one achieves n-1 trust, then there is no town judge
        return -1
```
```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        if (trust.length == 0 && n == 1) 
            return 1;
        
        // Create a list of of person and their trust value
        int[] count = new int[n + 1];

        // Increase trust value of trustee and decrease trust value of truster for each pair
        for (int[] person : trust) {
            count[person[0]]--;
            count[person[1]]++;
        }

        // Check if anyone achieves n-1 trust. This person is the town judge
        for (int person = 0; person < count.length; person++) {
            if (count[person] == n - 1) return person;
        }

        // If no one achieves n-1 trust, then there is no town judge
        return -1;
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `V` represents the number of vertices/nodes.
Assume `E` represents the number of edges

* **Time Complexity**: O(V+E) We will need to visit each vertex/nodes and their edges.
* **Space Complexity**: O(V) accounting for the use of a array to track the number of trust. 



