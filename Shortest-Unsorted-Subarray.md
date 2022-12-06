## Problem Highlights

* 🔗 **Leetcode Link:** [Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/) 
* 💡 **Problem Difficulty:** Mediumw
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we expect negative numbers in the input?
  - Yes, the input may contain negative numbers.

- Can we expect an empty array as input? What should we return in this case?
  - No, we will assume that there is at least one element in the input array.
   
```markdown
HAPPY CASE
Input: [1, 2, 5, 3, 7, 10, 9, 12]
Output: 5
Explanation: We need to sort only the subarray [5, 3, 7, 10, 9] to make the whole array sorted

Input: [1, 3, 2, 0, -1, 7, 10]
Output: 5
Explanation: We need to sort only the subarray [1, 3, 2, 0, -1] to make the whole array sorted

EDGE CASE
Input: nums = [1]
Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Arrays, there are some common techniques you can employ to help you solve the problem:
- Sort
    - Will sorting the array help you solve the problem?
- Two pointer solutions (left and right pointer variables)
    - If you are dealing with sorted arrays and need to find a set of elements that fulfill some constraint

- Storing the elements of the array in a HashMap or a Set
    - Will additional data structures help you solve the problem?

- Traversing the array with a sliding window
    - Will a restrictive window help us?
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use two pointers, one from the top the other from the bottom to find the first instance where numbers are out of order.

```markdown
1. Initialize LeftPtr at the start index and RightPtr at the last index
2. Iterate LeftPtr from left -> right as long as it points to elements in the increasing order. Break when arr[LeftPtr] < arr[index] (that's when you've spotted the first element that is out of order from left)
3. Iterate RightPtr from right -> left as long as it points to elements in the decreasing order. Break when arr[RightPtr] > arr[index] (that's when you've spotted the first element from right, that is out of order)
4. We now have found a candidate sub-array. Find the local min and local max in this sub array
5. Extend the sub-array from LeftPtr to the beginning of the array to include elements greater than the local min. This is to make sure that all the remaining elements to the left are indeed less than all elements in the subarray
6. Extend the sub-array from RightPtr to the end of the array to include elements smaller than the local max. This is to make sure that all the remaining elements to the right are indeed greater than all elements in the subarray
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def sort(arr):
    low = 0
    high = len(arr) - 1
    # find the first number out of sorting order from the beginning
    while low < len(arr) - 1 and arr[low] <= arr[low + 1]:
      low += 1

    if low == len(arr) - 1: # if the array is sorted
      return 0

    # find the first number out of sorting order from the end
    while high > 0 and arr[high] >= arr[high - 1]:
      high -= 1

    # find the maximum and minimum of the subarray
    subarrayMax = -float('inf')
    subarrayMin = float('inf')
    for k in range(low, high):
      subarrayMax = max(subarrayMax, arr[k])
      subarrayMin = min(subarrayMin, arr[k])

    # extend the subarray to include any number which is bigger than the minimum of the subarray 
    while low > 0 and arr[low - 1] > subarrayMin:
      low -= 1
    # extend the subarray to include any number which is smaller than the maximum of the subarray
    while high < len(arr) - 1 and arr[high + 1] < subarrayMax:
      high += 1

    return high - low + 1
```
```java
public int sort(int[] arr) 
  {
     //Step 1: Initialize the two Pointers
    int low = 0, high = arr.length - 1;
    
    // Step 2: find the first number out of sorting order from the beginning
    while (low < arr.length - 1 && arr[low] <= arr[low + 1])
      low++;

    if (low == arr.length - 1) // if the array is sorted
      return 0;

    // Step 3: find the first number out of sorting order from the end
    while (high > 0 && arr[high] >= arr[high - 1])
      high--;

    // Step 4: find the maximum and minimum of the subarray
    int subarrayMax = Integer.MIN_VALUE, subarrayMin = Integer.MAX_VALUE;
    for (int k = low; k <= high; k++) {
      subarrayMax = Math.max(subarrayMax, arr[k]);
      subarrayMin = Math.min(subarrayMin, arr[k]);
    }

    // Step 5: extend the subarray to include any number which is bigger than the minimum of the subarray 
    while (low > 0 && arr[low - 1] > subarrayMin)
      low--;
      
    // Step 6: extend the subarray to include any number which is smaller than the maximum of the subarray
    while (high < arr.length - 1 && arr[high + 1] < subarrayMax)
      high++;

    return high - low + 1;
  }
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), we need to access each number in the array
* **Space Complexity**: O(1), we simply need two pointers.