🔗 **Leetcode Link:** [https://leetcode.com/problems/maximal-network-rank](https://leetcode.com/problems/maximal-network-rank/discuss/?currentPage=1&orderBy=most_votes&query=)

⏰ **Time to complete**: __ mins

1. **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

    - How can you efficiently check if there is a road connecting two different cities?
    - Would it be efficient to use a matrix to save all the edges between vertex?
    - If the edge is close to `city_1` and `city_2`, then during counting, are they counted twice?
    
    ```markdown
    HAPPY CASE
    Input: n = 5, roads = [[0,1], [0,3], [1,2], [1,3], [2,3], [2,4]]
    Output: 5
    Explanation: There are 5 roads that are connected to cities 1 or 2. 
    
    EDGE CASE 
    Input: n = 8, roads = [[0,1], [1,2], [2,3], [2,4], [5,6], [5,7]]
    Output: 5
    Explanation: The network rank of 2 and 5 is 5. Notice that all the cities do not have to be connected.
    ```
    
2. M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

    - Use a hashtable of size `n` which stores sets for each city. Members of the sets are cities which are directly connected to the city the set corresponds to. Then, examine all unique pairs `(city_1, city_2)` and sum up the edges of both cities. If there is an edge between both cities, the sum needs to be reduced by one since the edge is counted twice. We store the highest sum of edges and return it.

3. P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
    ```
    1) Use a hashtable of size `n` which stores sets for each city. Members of the sets are cities which are directly connected to the city the set corresponds to.
    2) Then, examine all unique pairs `(city_1, city_2)`
    3) Sum up the edges of both cities.
    4) If there is an edge between both cities, the sum needs to be reduces by 1 since the edge is counted 2x.
    4) Then, store the highest sum of edges and return it.
    
    Time Complexity: O(M + N)
    Space Complexity: O(N)
    ```
    
    **Common Mistakes**
    
    - A common mistake would be iterating 2 times, in other words, 2 for loops, to find the 1st and 2nd maximum values while traversing the loop.
    - The 2 cities with most connections may not be necessarily connected with each other, and if they are connected, the common connection is counted only once.

4. I-mplement

> **Implement** the code to solve the algorithm.

    
    ```java
    class Solution {
        public int maximalNetworkRank(int n, int[][] roads) {
            Map<Integer, Set<Integer>> map = buildMap(n, roads);
            int ans = 0;
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (i != j) {
                        int sum = map.get(i).size() + map.get(j).size();
                        if (map.get(i).contains(j)) {
                            sum--;
                        }
                        ans = Math.max(ans, sum);
                    }
                }
            }
            return ans;
        }
        
        private Map<Integer, Set<Integer>> buildMap(int n, int[][] roads) {
            Map<Integer, Set<Integer>> map = new HashMap<>();
            for (int i = 0; i < n; i++) {
                map.put(i, new HashSet<>());
            }
            for (int[] road : roads) {
                map.get(road[0]).add(road[1]);
                map.get(road[1]).add(road[0]);
            }
            return map;
        }
    }
    ```
    
    ```python
    class Solution:
        def maximalNetworkRank(self, n: int, roads: List[List[int]]) -> int:
            city_to_cities = [ set() for i in range( n ) ]
            max_network_rank = 0
            for road in roads:
                city_to_cities[ road[0] ].add( road[1] )
                city_to_cities[ road[1] ].add( road[0] )
            for city_1 in range( n ):
                for city_2 in range( city_1 + 1, n ):
                    network_rank = len( city_to_cities[city_1] ) + len( city_to_cities[city_2] )
                    if ( city_1 in city_to_cities[city_2] ):
                        network_rank -= 1
                    max_network_rank = max(max_network_rank, network_rank)
            return max_network_rank
    ```
    
5. R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
6. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(M+N)`
<br>
Space Complexity: `O(N)`