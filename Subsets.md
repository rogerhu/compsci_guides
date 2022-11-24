## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/subsets/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Backtracking
* 🗒️ **Similar Questions**: [Subsets II](https://leetcode.com/problems/subsets-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is the input array assumed to be always sorted?
  - No, do not assume array is always sorted. 
- Does the solution print duplicates?
  - The solution set must not contain duplicate subsets.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: [5, 3, 9]
Output: [[],[5],[3],[3,5],[9],[5,9],[3,9],[3,5,9]]

Input: [1, 3, 4, 10, 9]
Output: [[],[1],[3],[1,3],[4],[1,4],[3,4],[1,3,4],[10],[1,10],[3,10],[1,3,10],[4,10],[1,4,10],[3,4,10],[1,3,4,10],[9],[1,9],[3,9],[1,3,9],[4,9],[1,4,9],[3,4,9],[1,3,4,9],[9,10],[1,9,10],[3,9,10],[1,3,9,10],[4,9,10],[1,4,9,10],[3,4,9,10],[1,3,4,9,10]]

EDGE CASE
Input: [10]
Output: [[],[10]]

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


* We are asked to calculate all the possible subsets, hence backtracking is an optimal algorithm because the strategy accepts the cases which satisfy conditions and reject the others. We basically loop over every element in our input nums, and we recursively call the method to generate subsets corresponding to that element in the next line and then we remove that element since we are done with it, and we add it to our subsets array.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Loop over the length of combination, rather than the candidate numbers, and generate all combinations for a given length with the help of backtracking technique.

```markdown
1) when the nums equals zero, it means there is zero elements to work, append them in the result and return.
2) choose not to include nums[0] in the subsets and explore all the subsets of nums[1:]
3) choose to include nums[0] in the subsets and explore all the subsets of nums[1:]
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

This problem is different than the rest of the usual backtracking practice problems in the sense that in this problem, we dont have a separate base case that tells us when to stop with the recursion. We keep looping until we run out of indexes. This will then mark the end of our recursion.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
	# use dictionary to store combinations
        self.res = {():True}
        self.helper((), nums)
        return self.res.keys()
        
    def helper(self, val, data):
        # if we have tried all numbers in the subset  - finish the recursion
        if not data: return
        
	# this loop helps us to create all possible combinations for numbers in data
        for i in range(len(data)):
            # if current index less then previous - means that we already has such 
            # subset but in another order. In that way we avoid sorting for checking 
            # if we already have this subset in the self.res
            if val and val[-1] > data[i]:
                continue
            new_val = val + (data[i], )
            # if we do not have that subset in the store - adding it
            # and tuple as a keys because it a great improving of speed instead of using list
            if new_val not in self.res:
                self.res[new_val] = True
                
            # run the same code for the new value and options for iteration.
            self.helper(new_val, data[:i]+data[i+1:])
        
```
```java
class Solution {
    
    // function to perform backtracking.
    static void Function(List<List<Integer>> list , List<Integer> permutation , int[] nums , int start)
    {
        // add new subset
        list.add(new ArrayList<>(permutation));
        
        for(int i=start;i<nums.length;i++)
        {
            permutation.add(nums[i]);
            // start from next index psition.
            Function(list,permutation,nums,i+1);
            // delete the last elment added.
            permutation.remove(permutation.size()-1);
        }
    }
    public List<List<Integer>> subsets(int[] nums) {
        
        // add all the possible subsets generated
        List<List<Integer>> list = new ArrayList<>();
        List<Integer> permutation = new ArrayList<>();
        
        Function(list,permutation,nums,0);
        
        return list;
        
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N * 2^N), to generate all subsets and then copy them into output list.
* **Space Complexity**: O(N). We are using O(N) space to maintain data array, and are modifying data array in-place with backtracking.