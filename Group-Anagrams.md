## Problem Highlights

* 🔗 **Leetcode Link:** 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, String
* 🗒️ **Similar Questions**: 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Is sorting a costly operation?
  - Yes, try using a hashmap or dictionary.
- What's the common factor of "abc", "acb" and "bca"?
  - "abc" is the KEY to group the anagrams.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: ["",""]
Output: [["",""]]

EDGE CASE
Input: strs = ["a"]
Output: ["a"]

Input: strs = [""]
Output: []
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement. We can use the sort technique to validate if two words are composed of the same characters set.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window 
Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition. This question involves the need to compare check whether each item in the array is an anagram of a previously read item. The approach to this question is quite straight forward — if the current item being read is an anagram of an item previously read, put them in the same array. If not, put it in a new array. To keep track of this, you can use a dictionary.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can transform each string \text{s}s into a character count, count, consisting of 26 non-negative integers representing the number of a's, b's, c's, etc. We use these counts as the basis for our hash map.

```markdown
1. Create a HashMap with values as List of strings where we will add the list of strings that are group of anagrams.
2. Create a for loop through the given array.
3. Convert the ith String to array and sort and back to string.
4. Check if the sorted string is present in hashmap, then add the ith string in hashmap else add the ith string in arraylist as value in hashmap
5. Return the values of hashmap as arraylist.
```

⚠️ **Common Mistakes**

* List comparison & iteration might take too long and too much space. Dictionaries or hashmap are great, low-cost data structures to utilize.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        # store the hashmap (characters counter) as a key and the same category str list as a value
        anagrams_lookup = {}
        result = []
        # go through the array and count the characters in each string
        for index, value in enumerate(strs):
            # each anagrams should have the same sorted substring (which is hashmap)
            key = ''.join(sorted(value))
            # adding substrings with the same anagram together in an array
            if key in anagrams_lookup:
                anagrams_lookup[key].append(value)
            else:
                anagrams_lookup[key] = [value]
        return anagrams_lookup.values()
```
```java
class Solution {
    Map<String, List<String>> data = new HashMap<>();
    List<List<String>> ans = new ArrayList<>();
    public List<List<String>> groupAnagrams(String[] strs) {
        // clasify word
        String sum = "";
        char[] num;
        // loop through the list group the same word and put to map
        for(String str : strs){
          num = new char[26];
            for(char c: str.toCharArray()){
                num[c-'a']++; 
            }
            sum = String.valueOf(num);
            if(!data.containsKey(sum)) {
                data.put(sum, new ArrayList<>());
            }
            data.get(sum).add(str);
            
        }
        // go back to list
        for(List<String> list: data.values()){
            ans.add(list);
        }
        return ans;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(NK), where N is the length of strs, and K is the maximum length of a string in strs.
* **Space Complexity**: O(NK), the total information content stored in ans

