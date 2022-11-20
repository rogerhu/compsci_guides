## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/largest-number/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Greedy
* 🗒️ **Similar Questions**: [Smallest Value of the Rearranged Number](https://leetcode.com/problems/smallest-value-of-the-rearranged-number/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we construct the largest number?
  - Ensure that the most significant digits are occupied by the largest digits.
- Why is it that when input is 2048, and "8420" is returned, that is the wrong answer?
  - You cannot arrange the individual numbers. you can only change the order of given numbers but not their digits.
- What are the constraints?
  - Constraints: 1 <= nums.length <= 100 and 0 <= nums[i] <= 109
   
```markdown
HAPPY CASE
Input: nums = [10,2]
Output: "210"

EDGE CASE
Input: nums = [3,30,34,5,9]
Output: "9534330"
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

* The Greedy Algorithm always choose the best at the current iteration. Given two numbers a and b, we pick the larger number if the string concatenation of a+b is bigger than b+a. If we compare any 2 non-overlapping substrings of some number x, we can determine what order the substrings must appear in x.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Sort the array, the most "signficant" number will be at the front.

```markdown
1. Convert each integer to a string. Then, we sort the array of strings.
2. Once the array is sorted, the most "signficant" number will be at the front. There is a minor edge case that comes up when the array consists of only zeroes, so if the most significant number is 00, we can simply return 00. Otherwise, we build a string out of the sorted array and return it.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

- If you are struggling to implement, try using a comparator. For example, a, b are the two strings obtained from the array passed into the sort() in java.

(a+b).compareTo(b+a) returns the smallest possible order.
(b+a).compareTo(a+b) returns the largest possible order.
If the zero flag case is not handled, only 226 out of 230 cases will pass. Suppose the array had only zeroes: [0,0]. Then, the string array would have ["0", "0"] and result would return "00" instead of "0". So, if all the elements in the array is 0, then simply return 0.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class LargerNumKey(str):
    def __lt__(x, y):
        return x+y > y+x
        
class Solution:
    def largestNumber(self, nums):
        largest_num = ''.join(sorted(map(str, nums), key=LargerNumKey))
        return '0' if largest_num[0] == '0' else largest_num
```
```java
class Solution {
    private class LargerNumberComparator implements Comparator<String> {
        @Override
        public int compare(String a, String b) {
            String order1 = a + b;
            String order2 = b + a;
           return order2.compareTo(order1);
        }
    }

    public String largestNumber(int[] nums) {
        // get input integers as strings.
        String[] asStrs = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            asStrs[i] = String.valueOf(nums[i]);
        }

        // sort strings according to custom comparator.
        Arrays.sort(asStrs, new LargerNumberComparator());

        // ff, after being sorted, the largest number is `0`, the entire number
        // is zero.
        if (asStrs[0].equals("0")) {
            return "0";
        }

        // build largest number from sorted array.
        String largestNumberStr = new String();
        for (String numAsStr : asStrs) {
            largestNumberStr += numAsStr;
        }

        return largestNumberStr;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(nlgn), the sort functionality in Python and Java is O(nlgn).
* **Space Complexity**: O(n), we allocate O(n) additional space to store the copy of nums.