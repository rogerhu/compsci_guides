## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/advantage-shuffle/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Greedy
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Why should we pair a and b if a > b?
  - Because every card in A is larger than b, any card we place in front of b will score a point. We might as well use the weakest card to pair with b as it makes the rest of the cards in A strictly larger, and thus have more potential to score points.
- Can we assume array nums2 is sorted?
  - Yes

Run through a set of example cases:

```markdown
HAPPY CASE
Input: nums1 = [[2,7,11,18], nums2 = [1,11,4,11]]
Output: [2,18,7,11]

Input: nums1 = [1,1,1,1], nums2 = [0,0,0,0]
Output: [1,1,1,1]

EDGE CASE
Input: nums1 = [1,0,0,1000000], nums2 = [0,0,0,0]
Output: [1000000,1,0,0]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


* A greedy solution can be applied here, where the current smallest card to beat in B will always be b = sortedB[j]. For each card a in sortedA, we will either have a beat that card b (put a into assigned[b]), or throw a out (put a into remaining). For each number n in A, the optimal solution is to find the minimum number in B m such that n > m.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** For each value in nums2, we ideally want to pick a number from nums1 that is just higher to match up against it. If we had a sorted nums2 as well, we could just match up the values very easily in descending order. Create an index order lookup array and sort it in reference to the values in nums2, then use it as a bridge between the sorted nums1 and unsorted nums2.

```markdown
1) count elements in A to a map m.
2) for each element in B, find the least bigger element in map m.
3) otherwise, we'll take the smallest element.
4) then we update the m.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

* The naive way to do this would require sorting A, then iterating through it until we find the ideal number, then removing that number from A and moving it to the answer array at a time complexity of O(n^2).
* If you employ binary search here instead of a straight iteration, which would drop the overall time complexity to O(n * log n), matching the sort time complexity. The issue that remains, however, is that getting rid of elements of A can be time-consuming.
* To keep elements sorted and to avoid comparing duplicate elements in B, consider using a map or a set to store a list of indices. You can "pop" the duplicated elements.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def advantageCount(A, B):
    sortedA = sorted(A)
    sortedB = sorted(B)

    # assigned[b] = list of a that are assigned to beat b
    # remaining = list of a that are not assigned to any b
    assigned = {b: [] for b in B}
    remaining = []

    # populate (assigned, remaining) appropriately
    # sortedB[j] is always the smallest unassigned element in B
    j = 0
    for a in sortedA:
        if a > sortedB[j]:
            assigned[sortedB[j]].append(a)
            j += 1
        else:
            remaining.append(a)

    # reconstruct the answer from annotations (assigned, remaining)
    return [assigned[b].pop() if assigned[b] else remaining.pop() for b in B]
```
```java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int n = nums1.length;
        
        int[] answer = new int[n];
		
	// we would have to store elements with indexes, so that we remember
	// where to place nums1 elements.
        List<int[]> list = new ArrayList<>();
        
        for(int i = 0; i < n; ++i) {
            list.add(new int[] {nums2[i], i});
        }
        
	// sort elements in descending order
        Collections.sort(list, (a, b) -> Integer.compare(b[0], a[0]));
		
	// sort in ascending order
	// we can use two pointers to pick elements from left and right ends of nums1
        Arrays.sort(nums1);
        
        for(int i = 0, left  = 0, right = n - 1; left <= right; ++i) {
            int[] current = list.get(i);
            
	    // right pointer will always point to the highest element of nums1 
	    // which isn't used yet
            if(current[0] < nums1[right]) {
                answer[current[1]] = nums1[right--];
            } else {
			
		// the easiest permutation would be to pick the smallest element,
		// as they would never contribute to increasing nums1's advantage over nums2.
                answer[current[1]] = nums1[left++];
            }
        }
        
        return answer;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N log N), where N is the length of A and B.
* **Space Complexity**: O(N)