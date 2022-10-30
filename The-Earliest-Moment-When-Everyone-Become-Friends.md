## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/>
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Union Find, Depth First Search
* 🗒️ **Similar Questions**: [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How can we sort the log items?
  - Sort the log items by their timestamp.
- How can we model this problem as a graph problem?
  - To keep track of connectivity of each element in the subset or connectivity of subsets with each other, you can use Union Find data structure to do this operation efficiently.
   
```markdown
HAPPY CASE
Input: logs = [[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[20190322,4,5]], n = 6
Output: 20190301

Input: logs = [[0,2,0],[1,0,1],[3,0,3],[4,1,2],[7,3,1]], n = 4
Output: 3

EDGE CASE
Input: logs = [[1,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[0,4,5]], n = 10
Output: -1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graph problems, we want to consider the following approaches:

- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. At the beginning we have a graph with N nodes but no edges. Then we loop through the events and if unite each node until the number of connected components reach to 1. Notice that each time two different connected components are united the number of connected components decreases by 1.
- DFS: Suppose we have a set. We can keep track of the friend circle and add people in the set. As soon as we find a new relation (by iterating through logs), we can check if this new relation will bring every member in set.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use union find to join two groups and reduce the group of friends by 1. At the end we will have only 1 friend group meaning everyone is friends with everyone.

```markdown
1. Initialize a variable time, which stores the value of the current timestamp of the DSU.
2. Traverse the given array, and perform union operation between and update the current timestamp to time if belong to the different sets.
3. If the total number of sets after completely traversing through the array is 1, return the value of the variable time, else return -1.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

You can think of the node "representing" the connected component as sometimes further (a certain number of edges) away. A common mistake is not seeing that the parent is closer. To ensure best runtime, the smaller connected component is merged into the larger one.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def earliestAcq(self, logs: List[List[int]], n: int) -> int:
        # sort the events in chronological order
        logs.sort(key = lambda x: x[0])

        uf = UnionFind(n)
        # treat each individual as a separate group
        group_cnt = n

        # merge the groups as we traverse the logs
        for timestamp, friend_a, friend_b in logs:
            if uf.union(friend_a, friend_b):
                group_cnt -= 1

            # return timestamp when individuals are connected to each other
            if group_cnt == 1:
                return timestamp

        # not everyone is connected
        return -1
```
```java
class Solution {
    public int earliestAcq(int[][] logs, int n) {
        // sort the events in chronological order
        Arrays.sort(logs, new Comparator<int[]>() {
            public int compare(int[] log1, int[] log2) {
                Integer tsp1 = new Integer(log1[0]);
                Integer tsp2 = new Integer(log2[0]);
                return tsp1.compareTo(tsp2);
            }
        });

        // treat each individual as a separate group
        int groupCount = n;
        UnionFind uf = new UnionFind(n);

        for (int[] log : logs) {
            int timestamp = log[0];
            int friendA = log[1];
            int friendB = log[2];
            // merge the groups
            if (uf.union(friendA, friendB)) {
                groupCount -= 1;
            }

            // return timestamp when individuals are connected to each other
            if (groupCount == 1) {
                return timestamp;
            }
        }

        // not everyone is connected
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

Let E be the number of edges or the number of timestamps.
Let V be the number of vertices or the number of people.

* **Time Complexity**: O(E * log(E) + EV) = O(E(log(E)+V)), as sorting logs takes O(E * log(E)) and going through each log takes O(E) and in each log we call union(with rank and find with which its worst case takes O(V) (amortized close to O(1)), thus the loop takes O(E * V)
* **Space Complexity**: O(V), as we used union find.