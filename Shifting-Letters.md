## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/shifting-letters/](https://leetcode.com/problems/shifting-letters/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, String
* 🗒️ **Similar Questions**: [Replace All Digits with Characters](https://leetcode.com/problems/replace-all-digits-with-characters/), [Shifting Letters II](https://leetcode.com/problems/shifting-letters-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Do we need to reverse iterate the string?
   - Since we need to calculate the prefix sum in reverse, yes. 
- How do we use ASCII to get value for shifted characters?
   - s[i]-'a' (gives us 97-97=0) . 'a' will now be represented by 0, b by 1 and so on.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: s = "abcd", shifts = [1, 3, 4, 5]
Output: "nnli"

EDGE CASE
Input: "abcd", shifts = [3, 5, 9, 1]
Output: "sqme"
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

We can calculate the number of shifts on each position, by shifts[i] += shift[i+1]. Then, we can go ahead and calculate the final string, by shift on each position shifts[i] times.

```markdown
1) Traverse the shift array from the end (n-1, where n is the length of an array) to start (index 0). 
2) Add value at the previous index to the value at the current index and take modulus by 26 (Total number of characters in English). This step is executed to find out the total number of shift operations to be performed on each character of the input string.
3) Create an array of characters from the input string.
4) Traverse the String array from the start (index 0) to end (n-1, where n is the length of an array). 
5) Perform the shift operation on each and every character of an array.
6) Convert modified array of characters to string and return it.
```

⚠️ **Common Mistakes**

* Don't forget to compute the suffix sum of shifts so that there is a one-to-one mapping from shifts to `s`. The trick here (otherwise it's time limit exceeded) is to aggregate all subsequent shifts for each letter. Making sure to modulo 26 so we don't surpass out integer size. Then, do the full shift.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def shiftingLetters(self, s: str, shifts: List[int]) -> str:
        res = ""
		
	# both lengths are equal
        length = len(shifts)
		
	# temp variable to store previous value to get sum for left chars
        tmp = 0
		
	# reverse loop to shift right most char 1st than sum shift of left chars one by one
        for i in range(length-1,-1,-1):
            shift = tmp + shifts[i]
            tmp = shift
            if shift > 26:
                shift = shift % 26
                
            res = self.getShiftedChar(ord(s[i]),shift) + res
        return res
    
    def getShiftedChar(self,char: int,shift: int) -> str: 
        asc = char
        asc += shift
		
        # until this ascii value is within required range, i.e, 97 to 122 
        while asc > 122:
            diff = asc - 122
            asc = 96 + diff
        char = chr(asc)
        return char
```
```java
class Solution {
    // a method to shift a lower-case char to 'n' position to the right
    public char shiftChar(char ch, int n){
        n %= 26;
        int move = (int) (ch + n);
        if(move > 122){
            ch = (char)('a' + move%122-1);
        }
        else{
            ch = (char) (ch + n);
        }
        return ch;
    }
    public String shiftingLetters(String s, int[] shifts) {
        // create an array of chars from the given string
        char str[] = s.toCharArray();
        
        // update all the elements of the 'shifts' array with the Postfix Sum of each index
        shifts[shifts.length-1] %= 26;              // update the last element by taking the mod by 26
        for(int i = shifts.length-2; i >= 0; i--)   // mod 26 is done so that sum doesn't exceed the max value of int
            shifts[i] += shifts[i+1]%26;            // add each element to the mod of 26 of its next element

        // then shift each char in the 'str' char array by the ith value of the 'shifts' array
        for(int i = 0; i < shifts.length; i++){
            str[i] = shiftChar(str[i], shifts[i]);
        }
        // finally, create a String from the updated char array and return it
        String newStr = new String(str);
        return newStr;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N), where N is the length of S (and shifts).
* **Space Complexity**: O(N), since an array was used
