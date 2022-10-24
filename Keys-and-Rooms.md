## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/keys-and-rooms/](https://leetcode.com/problems/keys-and-rooms/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: TBD

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

## 1: U-nderstand

- How do we keep track of the rooms? Does the set begin at room 0? If so, should I add 0 to the visited set?
- What if the room has not been visited yet? "If the key is not visited yet, add the key to visited and recursively visit keys in that room, otherwise do not visit the room again."
- When do we know all rooms are visited? "We can visit all rooms only when the size of visited set equals to the size of the rooms."
- What if inputs are too large? "If input is too large, DFS might cause stack overflow."
- How would you use DFS to see which rooms can be reached from room 0?
    
```markdown
    HAPPY CASE
    Input: [[1],[2],[3],[]]
    Output:	true
    
    EDGE CASE 
    Input: [[2], [], [1]]
    Output: false
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    To apply a graph algorithm for this problem, here are things we want to consider are:
    
    - Think about a breadth-first search (BFS) queue or a depth-first search (DFS) stack approach, or even a DFS recursion approach here. The DFS can be implemented in Recursion or the classic iterative approach with the help of a stack.
    - We would need a hash set e.g. unordered_set to remember the rooms that we have been to. Then, as long as we are in the room, we can depth first search the rooms whose keys are in the room. Once the search is finished, we can count the number of the keys in the set, and compare to the number of the rooms.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
    **Sample Approach:**
     Use a stack to store previous operator/operand combinations and compute the answer as we go.
    
```
    1) create a hashmap to hold graph that it will be a map of Integer: [], because we will insert room: [list of keys]
    2) build the map from the given list of lists
    3) create a boolean array to say whether a room is visited
    4) iterate over each room, perform DFS.
    5) for each room, push it onto stack
    6) while we still have rooms on stack (current room or rooms we could go to using the keys that we will find.
    7) finally, we iterate over all the rooms and see if we have any unvisited rooms
    
    Time Complexity: O(N + K)
    Space Complexity: O(N)
```
    
⚠️ **Common Mistakes**

* What if we start at the beginning and push the values into some array, then visit the cells of those inner values and push their values into the array, as well. We should end up with an array of length rooms.length if we get all the keys.
* Once DFS has completed (it will stop running once it can't find any more unvisited rooms), we check to see if its size is equal to the length of the rooms array.

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```python
    class Solution:
        def canVisitAllRooms(self, R: List[List[int]]) -> bool:
            vis, stack, count = [False for _ in range(len(R))], [0], 1
            vis[0] = 1
            while stack:
                keys = R[stack.pop()]
                for k in keys:
                    if not vis[k]:
                        stack.append(k)
                        vis[k] = True
                        count += 1
            return len(R) == count
```
    
```java
    class Solution {
        public boolean canVisitAllRooms(List<List<Integer>> rooms) {     
            Map<Integer, List<Integer>> graph = new HashMap<>();
            for(int i = 0; i < rooms.size(); i++) {
                graph.put(i, rooms.get(i));
            }
            boolean[] visited = new boolean[rooms.size()];
            Stack<Integer> s = new Stack<>();
            for(Integer room : graph.keySet()) {
                s.push(room);
    
                while(s.isEmpty() == false) {
                    Integer r = s.pop();
                    if (visited[r] != true) {
                        visited[r] = true;
                        for(Integer reachable: graph.get(r)) {
                            if (visited[reachable] != true)
                                s.push(reachable);
                        }
                    }
                }
                
                for(boolean b : visited)
                    if (b == false)
                        return false;
                
            }
            return true;
        }
    }
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: The time complexity for this algorithm is `O(N + K)`, where `N` is the number of rooms and `K` is the number of Keys.
<br>
Space Complexity: The space complexity is `O(N)` since we are using a list to store the visited rooms list.