## Problem Highlights

* 🔗 **Leetcode Link:** [Reverse Words in a String II](https://leetcode.com/problems/reverse-words-in-a-string-ii/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/), [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Does the input have leading or trailing spaces?
    - No


- Does input have more than a single space between a word?
    - All the words in the input are guaranteed to be separated by a single space.

- Can the input be empty?
    - The size range of the input is from 1 to 10^5. Therefore the input can never be empty


```markdown
HAPPY CASE
Input: s = ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]

Input: ["h","e","l","l","o"," ","w","o","r","l","d"]
Output: ["w","o","r","l","d"," ","h","e","l","l","o"]

EDGE CASE (Multiple Spaces)
Input: s = ["a"]
Output: ["a"]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array/Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem? We can not do this since we have to retain the order in this problem.


- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us. This solution can help us to maintain the order can solve this problem in place.


- Storing the elements of the array in a HashMap or a Set
    - A hashset will be not helpful here. We have to solve this problem in place, so this method will not help us.


- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us? This can not help solve this problem since we can not do anything even if we have a sliding window.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Reverse the Whole List and Then Reverse Each Word in the List




```markdown
1. Reverse the whole input List in place
2. Go through the input List
3. Determine where the word end - find each word in the input List
4. Reverse Each Word in the input List in place
```

⚠️ **Common Mistakes**

* Remember to use two pointer to achieve O(1) space

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def reverseWords(self, s: List[str]) -> None:
        # 1. Reverse the whole input List in place
        self.reverse(s, 0, len(s) - 1)
        
        # 4. Reverse Each Word in the input List in place
        self.reverse_each_word(s)

    def reverse(self, l: list, left: int, right: int) -> None:
        while left < right:
            l[left], l[right] = l[right], l[left]
            left, right = left + 1, right - 1
            
    def reverse_each_word(self, l: list) -> None:
        n = len(l)
        start = end = 0

        # 2. Go through the input List
        while start < n:
            # 3. Determine where the word end - find each word in the input List
            while end < n and l[end] != ' ':
                end += 1
            # reverse the word
            self.reverse(l, start, end - 1)
            # move to the next word
            start = end + 1
            end += 1
```
```java
class Solution {
    public void reverseWords(char[] s) {
        // 1. Reverse the whole input List in place
        reverse(s, 0, s.length - 1);

        // 4. Reverse Each Word in the input List in place
        reverseEachWord(s);
    }

    public void reverse(char[] s, int left, int right) {
        while (left < right) {
            char tmp = s[left];
            s[left++] = s[right];
            s[right--] = tmp;
        }
    }

    public void reverseEachWord(char[] s) {
        int n = s.length;
        int start = 0, end = 0;

        // 2. Go through the input List
        while (start < n) {
            // 3. Determine where the word end - find each word in the input List
            while (end < n && s[end] != ' ') ++end;
            // reverse the word
            reverse(s, start, end - 1);
            // move to the next word
            start = end + 1;
            ++end;
        }
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


* **Time Complexity**: O(n), we need to visit every item in the array to reverse the list and reverse each word in the list one more time. 
* **Space Complexity**: O(1), we only need several pointers. 
