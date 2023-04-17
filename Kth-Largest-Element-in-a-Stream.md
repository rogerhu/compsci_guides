## Problem Highlights

* 🔗 **Leetcode Link:** [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Array, Heap
* 🗒️ **Similar Questions**: [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/), [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be less than k elements in the array?
  - No, it is guaranteed that there will be at least k elements in the array when you search for the kth element.
- What is the time and space complexity?
    - Can you come up with an algorithm such that the add function has a O(logk) time complexity?


```markdown
HAPPY CASE
Input
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
Output
[null, 4, 5, 5, 8, 8]

Explanation
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8

Input
["KthLargest", "add", "add", "add", "add", "add"]
[[3,[4,5,8,2]],[10],[15],[10],[30],[4]]
Output
[null,5,8,10,10,10]

Explanation
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(10);   // return 5
kthLargest.add(15);   // return 8
kthLargest.add(10);  // return 10
kthLargest.add(10);   // return 10
kthLargest.add(10);   // return 10

EDGE CASE (Multiple Spaces)
Input
KthLargest kthLargest = new KthLargest(1, [4, 5, 8, 2]);
Output
[null,10,15,15,15,15]
kthLargest.add(10);   // return 10
kthLargest.add(15);   // return 15
kthLargest.add(10);  // return 15
kthLargest.add(10);   // return 15
kthLargest.add(10);   // return 15
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Does Two pointers help us find the kth largest item 
- Storing the elements of the array in a HashMap or a Set
    - A hashset will keep around items, but we still need to sort to find the kth largest item 
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?
- Heap
    - Adding to a heap will take O(logk) time.  

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create a min-heap and limit it's size to k. The first element will be the kth largest element


```markdown
1) Create heap
2) Limit heap to size k
3) Upon add(val), add new val to heap
4) Remove k + 1 largest val from heap
5) Return kth largest val in heap
```

⚠️ **Common Mistakes**

* Remember to use heap, we cannot get O(logk) otherwise

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        # Create heap
        self.heap = nums
        heapq.heapify(self.heap)

        # Limit heap to size k
        while len(self.heap) > k:
            heapq.heappop(self.heap)

    def add(self, val: int) -> int:
        # Upon add(val), add new val to heap
        heapq.heappush(self.heap, val)

        # Remove k + 1 largest val from heap
        heapq.heappop(self.heap)

        # Return kth largest val in heap
        return self.heap[0]
```
```java
class KthLargest {
	// Create heap
	private PriorityQueue<Integer> minHeap = new PriorityQueue<>();
	private int k;
	
	public KthLargest(int k, int[] nums) {
		this.k = k;
		for (int i: nums) {
			minHeap.add(i);

			// Limit heap to size k
			if (minHeap.size() > k) {
				minHeap.poll();
			}
		}
	}

	public int add(int val) {
		// Upon add(val), add new val to heap
		minHeap.add(val);

		// Remove k + 1 largest val from heap
		if (minHeap.size() > k) {
			minHeap.poll();
		}

		// Return kth largest val in heap
		return minHeap.peek();
	}
}
/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in array and `K` represents the number of items in the heap.


* **Time Complexity**: O(logK), each time we add an item to the heap, it may take O(logK) time. 
    *  Side note the init function take O(NlogN).
        *  O(N) to create the heap 
        *  O(N-K*logN-K) to remove N-K items from the heap. 
        *  If N and K are equal, then the init function take O(N) time. However, if K is 1, then the init function takes O(NlogN).  
* **Space Complexity**: O(K), we need to store `K` items in the heap