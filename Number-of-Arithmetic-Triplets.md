## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Arithmetic Triplets](https://leetcode.com/problems/number-of-arithmetic-triplets/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Hashtable
* 🗒️ **Similar Questions**: [Two Sum](https://leetcode.com/problems/two-sum/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do you calculate subsequent terms?
  - Observe that if you know the smallest term in an arithmetic triplet you can calculate the the subsequent terms.
- Are the elements in the array already sorted?
  - Yes. Because the numbers in original array are sorted, if for any index `i` we find `nums[i] + diff` and `nums[i] + (2 * diff)` in the set, we can be sure that they exist at indexes that satisfy the constraint of `i < j < k` since any value greater than current index number's value would be at a higher index than current index `(nums[i] + diff` and `nums[i] + (2 * diff)` will always be greater indexes than `nums[i]` since according to problem constraints `1 <= diff <= 50`).
   
```markdown
Example 1:

Input: nums = [0,1,4,6,7,10], diff = 3
Output: 2

Example 2:

Input: nums = [4,5,6,7,8,9], diff = 2
Output: 2
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:
* Sort. Sorting does not help solve this specific problem, because the ordering of the letters is important.
* Two pointer solutions (left and right pointer variables).Two pointer approach does not help solve this specific problem, because ordering of the letters is unidirectional. Take a variable count to keep a count of the triplets.
* Storing the elements of the array in a Hashtable. This could potentially work if know exactly what to look for after we store elements. Create dictionary where we are going to store the value and the index of each list element as a key-pair respectively. Then we iterate through the indices and values of the list containing our numbers. If the difference between the target and the current value in the list is already included as a key in the dictionary, then it means that the current value and the value stored in the dictionary is the solution to our problem. To speed up and simplify the looking up work, we can convert the `nums` array into a set (hashtable) data structure which has O(1) look up time complexity.
* Traversing the array with a sliding window. A solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.




## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1. Create a mapping of numbers to their positions. It suffices to perform two lookups in the mapping.
2. Verify that the positions satisfy the strict inequality from the problem statement.
```

**⚠️ Common Mistakes**

* Avoid misunderstanding the problem by reframing the problem: 
to find 3 numbers `(i, j , k)` such that `i < j < k`, `nums[j] - nums[i] == diff`, and `nums[k] - nums[j] == diff.` 

We can rearrange the last 2 constraints to define `nums[j]` and `nums[k]` in terms of `nums[i]` as follows:

```markdown
nums[j] - nums[i] == diff ---> nums[j] = nums[i] + diff

nums[k] - nums[j] == diff ---> nums[k] = nums[j] + diff ---> nums[k] = (nums[i] + diff) + diff) ---> nums[k] = nums[i] + (2 * diff)
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def arithmeticTriplets(self, nums: List[int], diff: int) -> int:
        ans = 0
        nums_to_pos = {num:index for index, num in enumerate(nums)}
        for i in range(len(nums)):
            a = nums[i]
            b, c = a + diff, a + 2 * diff
            if b in nums_to_pos and c in nums_to_pos:
                j, k = nums_to_pos[b], nums_to_pos[c]
                if i < j < k:
                    ans += 1
        return ans
```

```java
class Solution {
    public int arithmeticTriplets(int[] nums, int diff) {
        
        Set<Integer> set = new HashSet<>();
        int result = 0;
        for(int num : nums){
            if(set.contains(num-diff) && set.contains(num-(2*diff))){
                result += 1;
            }
            set.add(num);
        }
        return result;
            
      
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `n` is the number of elements, 

* **Time Complexity**: O(n) - we use two separate loops that iterate over the same number of elements

* **Space Complexity**: O(n) - we need to store the mapping from numbers to their positions in the array