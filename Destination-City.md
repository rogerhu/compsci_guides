## Problem Highlights

* 🔗 **Leetcode Link:** [Destination City](https://leetcode.com/problems/destination-city/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Hash Table
* 🗒️ **Similar Questions**: [Two Sum](https://leetcode.com/problems/two-sum), [Majority Element](https://leetcode.com/problems/majority-element)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be no solution for the input parameter ?
  - You may assume that each input would have exactly one solution.
- What is the time and space complexity?
    - Can you come up with an algorithm that is  O(n) time complexity?


```markdown
HAPPY CASE
Input: paths = [["London","New York"],["New York","Lima"],["Lima","Sao Paulo"]]
Output: "Sao Paulo" 
Explanation: Starting at "London" city you will reach "Sao Paulo" city which is the destination city. Your trip consist of: "London" -> "New York" -> "Lima" -> "Sao Paulo".

Input: paths = [["B","C"],["D","B"],["C","A"]]
Output: "A"
Explanation: All possible trips are: 
"D" -> "B" -> "C" -> "A". 
"B" -> "C" -> "A". 
"C" -> "A". 
"A". 
Clearly the destination city is "A".

EDGE CASE (Multiple Spaces)
Input: paths = [["A","Z"]]
Output: "Z"
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Does Two pointers help us find the destination city 
- Storing the elements of the array in a HashMap or a Set
    - A hashset will be helpful here, because we can store the start cities and end cities. Once we found a city in the end cities not in the start cities, we have found the destination city. 
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a hashset to store all the start-cities and another hashset to store all the end-cities. Check each end-city against the start-cities. The destination city is not in the start-cities.


```markdown
1) Create 2 hashsets
2) Iterate through the paths
3) Store the start and end cities.
4) Check each end-city against the start-cities
    1) If we do not see the end-city in the start-cities, then we found the destination city and return
```

⚠️ **Common Mistakes**

* Remember to use 2 hashset to store the start and end cities. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def destCity(self, paths: List[List[str]]) -> str:
        # Create 2 hashsets
        startCities, endCities = set(), set()

        # Iterate through the paths
        for startCity, endCity in paths:
            # Store the start and end cities.
            startCities.add(startCity)
            endCities.add(endCity)

        # Check each end-city against the start-cities
        for endCity in endCities:
            # If we do not see the end-city in the start-cities, then we found the destination city and return
            if endCity not in startCities:
                return endCity
```
```java
class Solution {
    public String destCity(List<List<String>> paths) {
        // Create hashsets
        Set<String> cities = new HashSet<>(); 

        // Iterate through the paths
        for (List<String> path : paths) {
            cities.add(path.get(0)); 
        }
        
        // Check each end-city against the start-cities
        for (List<String> path : paths) {
            String dest = path.get(1); 

            // If we do not see the end-city in the start-cities, then we found the destination city and return
            if (!cities.contains(dest)) {
                return dest; 
            }
        }
        return "";
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of paths in the array.


* **Time Complexity**: O(n), we need to visit every path in the array.
* **Space Complexity**: O(n), we need to  build a hashset of each startCity and endCity.