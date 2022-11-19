## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/max-chunks-to-make-sorted-ii/](https://leetcode.com/problems/max-chunks-to-make-sorted-ii/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array
* 🗒️ **Similar Questions**: [Max Chunks To Make Sorted](https://leetcode.com/problems/max-chunks-to-make-sorted/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Could the input array be null?
   - Let’s assume the input array is not null. In addition, the input array should have at least 1 element.
- Could the input numbers be negative?
   - Yes the input numbers could be positive, negative, or zero.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: [5,4,3,2,1]
Output: 1

HAPPY CASE
Input: [2,1,3,4,4]
Output: 4

EDGE CASE
Input: [1]
Output: 1
```   
    
## 2: M-atch

> **Match** 

In Strings/Arrays, common problem patterns include:

- Hash and Store: At each index, what information do we need? A potential solution could store the minimum number to the right of each index, key is the index. We could potentially hash that information; however, an array could serve the same purpose.
- Two Pointer: Two pointer may help us reference both sides of the array at the same time. However, since the array is not sorted, a two pointer solution may not assist us as much.
- Sort: How do we know when to split a chunk? If we know the underlying strategy on when to split a chunk (which relies on the sorted version of the data), sorting may potentially help us.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create an array that contains the minimum element to the right of a given index. Use this array to justify where we create a chunk.

```markdown
1) Generate an array that stores the minimum value to the right of a given index
2) Create two variables to store the running max value from the left and 
    number of chunks, number of chunks to 1 by default
3) Iterate through both arrays in parallel
    a) Update the maximum based on the original array.
    b) If the maximum from the left (inclusive) is less than or equal to the 
        minimum from the right, we create a chunk there because there is no value 
        to the right of the current subarray that would be required to be sorted 
        with the left chunk.
        i) By "create a chunk", increment the chunk variable
    c) Else, continue growing this chunk
4) Return number of chunks
```

⚠️ **Common Mistakes**

* Some people may get confused why the question asks for the maximum number of chunks rather than minimum. Take time to explain to the student that the minimum number of chunks for any array is 1, and that problem is trivial, since the answer is always 1.
* Some people may not clearly understand the potential for duplicate elements. There are two variants of the problem. This is the problem with duplicates.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int[] leftMax=new int[arr.length];    // this array stores the max value from 0 to ith index at ith index
        leftMax[0]=arr[0];                     
        int[] rightMin=new int[arr.length];   // this array stores the min value from i to arr.length-1 at ith index
        rightMin[arr.length-1]=arr[arr.length-1];
        for(int i=1;i<arr.length;i++){
            leftMax[i]=Math.max(leftMax[i-1],arr[i]);
        }
        for(int i=arr.length-2;i>=0;i--){
            rightMin[i]=Math.min(rightMin[i+1],arr[i]);
        }
        
        // now we traverse in leftMax array and rightMin array and compare the ith value of leftMax to (i+1)th value of rightMin array       
        int i=0;
        int count=0;  // count of number of partitions
        while(i<arr.length-1){
            if(leftMax[i]<=rightMin[i+1]){
                count++;
            }
            i++;
        }
        
        // the number of chunks is one more than the number of partitions 
        return count+1;        
        
    }
}
```
```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int count=0;
        for(int i=0;i<arr.length;){
        int num=arr[i];
            int max=arr[i];//current max
           for( int j=i+1;j<arr.length;j++){
               if(arr[j]<num){
                   i=j;
                   num=max;//increase the range
               }
               else{
                   max=Math.max(arr[j],max);//max element till now
               }
           }
            i++;
            count++;
        }
        return count;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where is the length of the array
* **Space Complexity**: O(n) to account for the use array
