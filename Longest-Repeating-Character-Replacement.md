## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/longest-repeating-character-replacement/](https://leetcode.com/problems/longest-repeating-character-replacement/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Sliding Window, String
* 🗒️ **Similar Questions**: [Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/), [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/), [Maximize the Confusion of an Exam](https://leetcode.com/problems/maximize-the-confusion-of-an-exam/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- If I am using the sliding window to solve this problem, what are some constraints?
  - Solve under the constraint that the window size does not decrease
- Why is the right bound increased?
  - Because the size of the sliding window does not decrease
- How can we divide the string?
  - We can divide all the characters of the string `s` into two groups - fixed letters and the letters that will be changed. Fixed letters remain unchanged. 

Run through a set of example cases:

```markdown
HAPPY CASE
Input: "ABAB", k = 2
Output: 4

EDGE CASE
Input: "AAAABBBBB", k = 2
Output: 7
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition. By implementing a sliding window, we can have the right edge increase 1 at each step. At each step, we get the sliding window size r - l + 1, we also update the character frequency freq[c] and find the character with the highest frequency maxLen, for all the characters in the sliding window. If r -l + 1 - maxLen <= k, we are able to change all the other characters in the sliding window to the highest frequency character. As a result, we update the result for the current window size, then go to the next step. If r -l + 1 - maxLen > k, we should stop expanding, and shrink the sliding window to restore the constraint r -l + 1 - maxLen <= k. We move the left edge one step at a time. At each step, we get the sliding window size r - l + 1, we also update the character frequency freq[c] and find the character with the highest frequency maxLen, for all the characters in the sliding window. This could provide a complexity of O(N).

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** The initial step is to extend the window to its limit, that is, the longest we can get to with maximum number of modifications. Until then the variable start will remain at 0. Use sliding window to update start accordingly. 

```markdown
1. Initialize start = 0 and end = -1. They represent the indexes of the window's left most and the most characters respectively.
2. Initialize a hash map frequencyMap to contain characters and their frequencies.
3. Initially the size of the window is 0. Expand the window by moving end pointer forward. We do so until the window becomes invalid.
4. Every time end moves forward, we update the frequency map of the newly added element. We update maxFrequency if its frequency is the maximum we have seen so far.
5. If the window is invalid, move the start pointer ahead by one step.
6. We repeat the last two steps until the window reaches the right edge of the string.
```

⚠️ **Common Mistakes**

* The initial step is to extend the window to its limit, that is, the longest we can get to with maximum number of modifications. Until then the variable start will remain at 0. Then as end increase, the whole substring from 0 to end will violate the rule, so we need to update start accordingly (slide the window). We move start to the right until the whole string satisfy the constraint again. Then each time we reach such situation, we update our max length.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        start = 0
        frequency_map = {}
        max_frequency = 0
        longest_substring_length = 0
        for end in range(len(s)):
            frequency_map[s[end]] = frequency_map.get(s[end], 0) + 1

            # the maximum frequency we have seen in any window yet
            max_frequency = max(max_frequency, frequency_map[s[end]])

            # move the start pointer towards right if the current
            # window is invalid
            is_valid = (end + 1 - start - max_frequency <= k)
            if not is_valid:
                frequency_map[s[start]] -= 1
                start += 1

            # the window is valid at this point, store length
            # size of the window never decreases
            longest_substring_length = end + 1 - start

        return longest_substring_length
```
```java
class Solution {
    public int characterReplacement(String s, int k) {
        int start = 0;
        int[] frequencyMap = new int[26];
        int maxFrequency = 0;
        int longestSubstringLength = 0;

        for (int end = 0; end < s.length(); end += 1) {
            int currentChar = s.charAt(end) - 'A';
            frequencyMap[currentChar] += 1;
            // the maximum frequency we have seen in any window yet
            maxFrequency = Math.max(maxFrequency, frequencyMap[currentChar]);

            // move the start pointer towards right if the current
            // window is invalid
            Boolean isValid = (end + 1 - start - maxFrequency <= k);
            if (!isValid) {
                // offset of the character moving out of the window
                int outgoingChar = s.charAt(start) - 'A';

                // decrease its frequency
                frequencyMap[outgoingChar] -= 1;

                // move the start pointer forward
                start += 1;
            }

            // the window is valid at this point, note down the length
            // size of the window never decreases
            longestSubstringLength = end + 1 - start;
        }

        return longestSubstringLength;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where n is the number of characters in the string
* **Space Complexity**: 0(m), where m is represents if there are m unique characters, then the memory required is proportional to m. 
