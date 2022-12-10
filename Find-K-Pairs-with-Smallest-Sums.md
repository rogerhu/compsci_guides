## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/find-k-pairs-with-smallest-sums/>
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 10 to 12 mins
* 🛠️ **Topics**: Heap
* 🗒️ **Similar Questions**: [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What kind of heap can we use?
  - For finding the k smallest pairs, we will need a max-heap of size k. The idea is to take all combinations of elements from both lists, and keep adding them to a max-heap until it becomes full.
- What do we compare once the max heap is full?
  - Once the max heap is full, we compare the sum of the new pair with the top element in max heap.
- When do we pop and add our new element?
  - Only if the new sum is lesser than the sum of the max pair in the heap

Run through a set of example cases:

```markdown
HAPPY CASE
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]

Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
Output: [[1,1],[1,1]]

EDGE CASE
Input: nums1 = [1,2], nums2 = [3], k = 3
Output: [[1,3],[2,3]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Heaps: In the heap data structure, we assign key-value or weight to every node of the tree. Now, the root node key value is compared with the children’s nodes and then the tree is arranged accordingly into two categories i.e., max-heap and min-heap. Heap data structure is basically used as a heapsort algorithm to sort the elements in an array or a list. 
- Multiple passes: To find the length, or save other information about the contents. If we were able to take multiple passes of the linked list, would that help solve the problem? Multiple passes would not help this problem since we aren’t collecting unique items or need any pre-processing.
- Two pointers: ‘Race car’ strategy with one regular pointer, and one fast pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
A slow and fast pointer may not help the problem.
- Dummy node: Helpful for preventing errors when returning ‘head’ if merging lists, deleting from lists. Would using a dummy head as a starting point help simplify our code and handle edge cases? Since we have to manipulate the order of the list, including the head of the list, a dummy head would help maintain a pointer to beginning of the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** For finding the `k` smallest pairs, use a max-heap of size `k`. The idea is to take all combinations of elements from both lists, and keep adding them to a max-heap until it becomes full.

```markdown
1) Initialize a heap to store the sum of pairs and their respective indexes.
2) Initially, we start making pairs by pairing only the first element of the second list with each element of the first list. We push the pairs onto a max-heap, sorted by the sum of each pair.
3) Use another loop to pop the smallest pair from the max-heap, noting the sum of the pair and the list indexes of each element, and add the pair to a result list.
4) To make new pairs, we move forward in the second list and pair the next element in it with each element of the first list, pushing each pair on the max-heap.
5) We keep pushing and popping pairs from the max-heap until we have collected the required k smallest pairs in the result list.
```

⚠️ **Common Mistakes**

*  Is it possible for `nums1[0]` + `nums2[k+1]` can be a candidate? This cannot be, because `nums1[0] + nums2[0....k]` is always smaller than `nums1[0] + nums2[k+1]`. Each time after we pick the pair with min sum, we put the new pair with the second index +1. ie, pick (0,0), we put back (0,1). Therefore, the heap alway maintains at most min(k, len(nums1)) elements.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def kSmallestPairs(self, nums1: List[int], nums2: List[int], k: int) -> List[List[int]]:
    heap = []
    nums1.sort()
    nums2.sort()
    
    for n1 in nums1:
        # if this condition occurs, that means whatever number will be larger than any element
        # on the heap
        if len(heap) >= k and n1 + nums2[0] > -heap[0][0]:
            break
        for n2 in nums2:
            sm = n1 + n2
            if len(heap) < k:
                heapq.heappush(heap, (-sm, (n1, n2)))
            elif sm < -heap[0][0]:
                    heapq.heappop(heap)
                    heapq.heappush(heap, (-sm, (n1, n2)))
            else:
                # greater than the current
                # jth loop so no need to check it
                break
    
    ls = []
    for ele in heap:
        ls.append(ele[1])
        
    return ls      
```
```java
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        // we set a pq to store the minimum sum
        PriorityQueue<List<Integer>> pq = new PriorityQueue<>((i1, i2) -> ((i1.get(0) + i1.get(1)) - (i2.get(0) + i2.get(1))));

        // try first k elements in nums1 here
        for(int i = 0; i < Math.min(nums1.length, k); i++){
            pq.add(new ArrayList<Integer>(Arrays.asList(nums1[i], nums2[0], 0)));
        }
        
        List<List<Integer>> ret = new ArrayList<>();

        while(!pq.isEmpty() && k > 0){
            List<Integer> cur = pq.poll();
            ret.add(new ArrayList<>(Arrays.asList(cur.get(0), cur.get(1))));
            k--;
            
            // because we only do the traverse in nums1
            // so we sum the current smallest element in nums1
            // with the current searched smallest element in nums2
            if(cur.get(2) + 1 < nums2.length){
                pq.offer(new ArrayList<Integer>(Arrays.asList(cur.get(0), nums2[cur.get(2) + 1], cur.get(2) + 1)));
            }
        }
        
        return ret;
    }
}   
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n*logn) assuming k<=n
* **Space Complexity**: O(n) as we are using extra space
