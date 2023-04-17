## Problem Highlights

* 🔗 **Leetcode Link:** [Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Array, Hash Table
* 🗒️ **Similar Questions**: [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/), [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/), [Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input array be empty?
    - No. There must be at least a single number in the array of numbers. 

   
```markdown
HAPPY CASE
Input: arr = [1,2,2,1,1,3]
Output: true
Explanation: The value 1 has 3 occurrences, 2 has 2 and 3 has 1. No two values have the same number of occurrences.

Input: arr = [1,2]
Output: false

EDGE CASE
Input: arr = [1]
Output: true
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - We can sort the array of numbers and check if the next item is equal to the prev item. Once we find a match, we can tally the number. However this is O(nlogn) time for the sort. 
- Two pointer solutions (left and right pointer variables). 
    - We will need to sort the array first to use the two pointer solution, but it really isn't all that helpful when sort does the job.
- Storing the elements of the array in a HashMap or a Set. 
    - As we iterate through the array, we can store each number in a Hash Table and count the occurance of each number. Once we move through the array, we know if there are unique number of occurrences of each number 
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - We will need to sort the array first to use the sliding window, but it really isn't all that helpful when sort does the job.

**⚠️ Common Mistakes**

* Sorting requires a O(NlogN) runtime, while a Hash requires a O(N) runtime. If we want to speed up our algorithm we should use a hash.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create a Hash Table and count the occurance of each number. Once we move through the array, we know if there are unique number of occurrences of each number 


```markdown
1) Initialize Hash Table
2) Iterate through numbers
    a) Count each number
3) Return if duplicate values exist in hash table
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        # Initialize Hash Table
        hashTable = defaultdict(int)

        # Iterate through numbers: Count each number
        for num in arr:
            hashTable[num] += 1
        
        # Return if duplicate values exist in hash table
        return len(list(hashTable.values())) == len(set(hashTable.values()))
```
```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) 
    {
        // Initialize Hash Table
        HashMap<Integer,Integer> hmap=new HashMap<>();

        // Iterate through numbers: Count each number
        for(int i:arr)
            hmap.put(i,hmap.getOrDefault(i,0)+1);

        // Return if duplicate values exist in hash table
        HashSet<Integer> hset=new HashSet<>(hmap.values());
        return hset.size()==hmap.size();
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(N)` because we need to traverse all numbers in the array.
* **Space Complexity**: `O(N)` because we need to store all numbers in the hash table. 