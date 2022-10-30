## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/course-schedule/](https://leetcode.com/problems/course-schedule/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/), [Course Schedule III](https://leetcode.com/problems/course-schedule-iii/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- When should the program return false? 
  - We only return false when we encounter a cycle. We encounter a cycle when course A needs course B and course B needs course A. So we just need to write a function to check if course A needs course B and course B needs course A. As long as there is no cycle, we can complete all the courses.
    
- Can you take a course without a prerequisite?
  - There is no problem taking up courses for which there is no prerequisite. For the other courses, as long as there is no cyclic dependency, we can finish them. 
    
- Do we need track completed courses? 
  - Maintain a list of completed courses to remind us that these courses have already been verified to not form a cycle. This is to avoid doing DFS over and over again on the same courses.
    
- What are the three course statuses?
  - For each class there are 3 statuses: not visited, visiting, visited.  
    
```markdown
    HAPPY CASE
    Input: numCourses = 4, prerequisites = [[0,1], [1,2], [2,3]]
    Output: true
    Explanation: In this case it is possible, just take 0 -> 1 -> 2 -> 3
    
    Input: numCourses = 4, prerequisites = [[0,1], [2,1], [2,3], [3,0]]
    Output: true
    Explanation: In this case it is possible, just take 2 -> 3 -> 0 -> 1
    
    EDGE CASE
    Input: numCourses = 5, prerequisites = []
    Output: true
    Explanation: In this case, no courses have prereqs, so we can take them in any order
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- DFS: We can use DFS to solve this problem. Utilize an adjacency list with key as course and value of list of prerequisites needed before taking this course. If node `v` has not been visited, then mark it as `0`. If node `v` is being visited, then mark it as `1`. If we find a vertex marked as `1` in DFS, then there is a ring. If node `v` has been visited, then mark it as `+1`. If a vertex was marked as `1`, then no ring contains `v` or its successors.
- BFS: We cannot use BFS to traverse the graph because we may visit exit nodes in the first traversal.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Only return false when we encounter a cycle
    
    **Approach #1**
    
    - Build a graph out of the prereq-course pairs
        - Every course is a node in the graph, and for a pair [a, b], add a directed edge from b to a, indicating that b depends on a
        - We can represent this via an adjacency list
    - Traverse the graph
        - We have to start with a node that has 0 prereqs, which graphically would be a node with outdegree 0
        - Once that node is fulfilled, we can remove that node from the graph to indicate that it has been taken
        - Then, we can repeat to find the next class we are eligible to take
        - If by the end we have taken all the classes, return true
        - If we reach a point where there are no eligible classes to take, return false
    
    **Approach #2:** Use an adjacency list
    
    - Ad list would look like this: ad_list = { 0: [3], 1: [0, 2], 2: [], 3: [2] }
    - First iteration, len(ad_list[2]) == 0, so we know 2 does not have any dependencies, so take course 2, and remove any nodes pointing to 2, and remove 2
        - ad_list = { 0: [3], 1: [0], 3: [] }
    - Second iteration, len(ad_list[3]) == 0, so we know 3 has no dependencies, so take course 3
        - ad_list = { 0: [], 1: [0] }
    - Third iteration, len(ad_list[0]) == 0, so take course 0
        - ad_list = {1: []}
    - Fourth iteration, take course 1
    - ad_list is empty, so we have taken all courses


⚠️ **Common Mistakes**

* This problem is equivalent to finding if a cycle exists in a directed graph. If a cycle exists, no topological ordering exists and therefore it will be impossible to take all courses. Don't mistake the definition for indegree. When offer into queue at first, you should add for those courses which needs precouse. Thus, their indexes are all reversed.


## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
**Approach #1**
    
```java
    class Solution {
        Boolean[] completedCourses;
        public boolean canFinish(int numCourses, int[][] prerequisites) {
            completedCourses = new Boolean[numCourses];

            // build an adjacency list
            List<Integer>[] adjList = (ArrayList<Integer>[]) new ArrayList<?>[numCourses];
            for(int[] e : prerequisites){
                if(adjList[e[1]] == null)
                    adjList[e[1]] = new ArrayList<>();
                adjList[e[1]].add(e[0]);
            }
            
            for(int i=0; i < numCourses; i++){
                if(isCycle(i, adjList, new boolean[numCourses]))
                    return false;
            }
            return true;
        }
        
        // check if there is a cycle
        private boolean isCycle(int course, List<Integer>[] adjList, boolean[] visited){
            if(visited[course])
                return true;
            if(completedCourses[course] != null){
                return false;
            }
            completedCourses[course] = true;
            visited[course] = true;
            
            List<Integer> nextCourses = adjList[course];
            if(nextCourses != null){
                for(int nextCourse : nextCourses){
                    if(isCycle(nextCourse, adjList, visited))
                        return true;
                }
            }
            visited[course] = false;
            return false;
        }
    }
```
    
```python
    class Solution:
        def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:

            # create adjacency list
            ad_list = {i: set() for i in range(numCourses)}
            for prereq, course in prerequisites:
                // add prereq to adjacency list
                ad_list[course].add(prereq)
            
            // check if there is a cycle
            while len(ad_list) > 0:
                next_course = None
                for course in ad_list:
                    if len(ad_list[course]) == 0:
                        next_course = course
                        break
                
                if next_course == None:
                    return False
    
                for course in ad_list:
                    ad_list[course].discard(next_course)
                
                ad_list.pop(next_course)
            
            return True
```
    
**Approach #2:** This approach creates a graph out of the prereq-course pairs, and attempts to topologically sort the graph into the `eligibleCourses` array.
    
```java
    # Java Solution
    class Solution {
        public boolean canFinish(int numCourses, int[][] prerequisites) {
            int[] indegree = new int[numCourses];
      
            for(int pre[] : prerequisites){
                indegree[pre[0]]++;
            }

            // create queue
            Queue<Integer> q = new LinkedList<Integer>();
          
            for(int i=0;i<indegree.length;i++){
                if(indegree[i] == 0)
                    q.add(i);
            }
         
            if(q.isEmpty())
                return false;
            while(!q.isEmpty()){
                int course = q.poll();
                for(int pre[]: prerequisites){
                    if(pre[1] == course){
                        indegree[pre[0]]--;
                        if(indegree[pre[0]] == 0)
                            q.add(pre[0]);
                    }
                }
            }

            // if there are still some edges left, then there exist some cycles
            // due to dependencies, we cannot remove the cyclic edges
            for(int a: indegree){
                if(a != 0)
                    return false;
            }
            return true;
        }
    }
```
    
```python
    # Python Code
    class Solution:
        def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
            ad_list = {i: set() for i in range(numCourses)}
            degrees = [0] * numCourses
            for prereq, course in prerequisites:
                ad_list[prereq].add(course)
                degrees[course] += 1
            
            eligibleCourses = []
            for course in range(numCourses):
                if degrees[course] == 0:
                    eligibleCourses.append(course)
            
            i = 0
            while i < len(eligibleCourses):
                next_course = eligibleCourses[i]
    
                for course in ad_list[next_course]:
                    degrees[course] -= 1
                    if degrees[course] == 0:
                        eligibleCourses.append(course)
                
                i += 1
            
            return len(eligibleCourses) == numCourses
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity:** O(V^2)
    - Creating the adjacency list takes O(E) time, where E is the number of prereqs (edges in the graph)
    - On each iteration we take one course, so at most there are n iterations, where V = numCourses
        - On each iteration, we iterate over the list twice, which takes O(V) time
    - Therefore, the total time is O(V^2) + O(E)

* **Space Complexity:** OV + E)
    - Since we can have at most V*(V-1) prereqs, E < V^2, so total time is O(V^2)
    - Space is also O(V + E) = O(V^2) due to the space needed for adjacency list