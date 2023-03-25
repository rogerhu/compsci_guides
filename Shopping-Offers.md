## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/shopping-offers/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What if a promotion is offered?
  -  We check each promotion offers. If the offer can be used, use it. Repeat the process and find the minimum result. In this question, the condition whether one offer can be used is the number of items in the offer doesn't exceed the needed number. Find the minimum among all combinations.

- Why should we start at the zeroth index?
  - Start with 0th index. At each index of offers we have two choice. Either we use that offer or we don't use that.
Choice 1: First check if we can use that offer if we can then pick that offer present on that index and stay on that index because we can use a offer any number of times.
Choice 2: Don't use that offer and move to next offer to do the same.

   
```markdown
Example 1:

Input: price = [2,5], special = [[3,0,5],[1,2,10]], needs = [3,2]
Output: 14
Explanation: There are two kinds of items, A and B. Their prices are $2 and $5 respectively. 
In special offer 1, you can pay $5 for 3A and 0B
In special offer 2, you can pay $10 for 1A and 2B. 
You need to buy 3A and 2B, so you may pay $10 for 1A and 2B (special offer #2), and $4 for 2A.

Example 2:

Input: price = [2,3,4], special = [[1,1,0,4],[2,2,1,9]], needs = [1,2,1]
Output: 11
Explanation: The price of A is $2, and $3 for B, $4 for C. 
You may pay $4 for 1A and 1B, and $9 for 2A ,2B and 1C. 
You need to buy 1A ,2B and 1C, so you may pay $4 for 1A and 1B (special offer #1), and $3 for 1B, $4 for 1C. 
You cannot add more items, though only $9 for 2A ,2B and 1C.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

We can see that this problem resembles a multidimensional knapsack problem, which we know to be NP-hard, and the small input size constraints seem to confirm that we may be dealing with an exponential-time solution. One approach here that comes to mind is that of backtracking. In this problem, we know what our possible candidates are: at each step in our backtracking process, our next move could be to either use the original item prices, or to use an offer.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.


```markdown
If we consider a recursive backtracking depth-first search approach, we would follow something like this:

1. For any given state, if it is a solution, report it. Otherwise...
2. Construct the set of possible candidates for the next choice
3. For each candidate, make a move that modifies the state
4. Using this new state, recur to continue backtracking
5. After returning from this recursion, unmake the move to undo any state modification
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

For making a move, we need to be careful with efficiency. When we make a move, it just boils down to buying some quantity of some items, so we can remove those items from the needs vector. We could try clone the vector and modify the clone, then pass this clone down to recursive calls, but this will require space proportional to the needs size multiplied by the recursion tree height. Instead, we'll opt for an approach that doesn't clone the vector thus saving space: before recurring deeper, modify the original vector to its new state. This is fine, since we'll never need the previous state in any children calls. Then once we finish that recursion, we can simply undo our vector modifications. Through all recursion then, simply pass a reference to the original vector.

When doing memoization in this way, one approach would be to use some sort of associative map to relate the state (key) to the resulting min_cost (value). In this case, our key would be the needs vector, but we need a way of producing a hash of this vector to act as a lookup key. An effective approach here is to build a bitmask: we know that needs can have at most 6 elements, and each of these elements is between 0 and 10 inclusive. So, each element can be represented using 4 bits, and so all 6 elements can be represented using 6*4=24 bits, which means we can store our entire state in a single 32-bit integer, which will become our bitmask.
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def shoppingOffers(self, price, special, needs):
        
        def recursion(price, special_offers, needs, current_items, n):
            n = len(price)
            count = 0
            purchase_cost_if_bought_without_any_offer = current_items[-1]
            for i in range(n):
                if current_items[i] > needs[i]:
                    return
                elif current_items[i] == needs[i]:
                    count += 1
                else:
                    purchase_cost_if_bought_without_any_offer += (needs[i] - current_items[i]) * price[i]
            
            # If we already purchased what we needed
            if count == n:
                self.ans = min(self.ans, current_items[-1])
                return
            else:
                self.ans = min(self.ans, purchase_cost_if_bought_without_any_offer)
            
            for i in range(len(special_offers)):
                # Using offer one by one and adding items coming in that offer to our current_items list. 
                for j in range(len(current_items)):
                    current_items[j] += special_offers[i][j]
                recursion(price, special_offers, needs, current_items, n)
                # backtrack
                for j in range(len(current_items)):
                    current_items[j] -= special_offers[i][j]

        # total items 
        n = len(price)
        # Create an array to store number of n items purchased till now. Use the last index to store the cost for purchasing current set of items
        current_items = [0 for _ in range(n + 1)]
        self.ans = sys.maxint
        recursion(price, special, needs, current_items, n)
        return self.ans   
        
```
```java
class Solution {
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        HashMap<String, Integer> dp = new HashMap<>();
        return backtracking(price, special, needs, dp);
    }
    int backtracking(List<Integer> price, List<List<Integer>> special, List<Integer> needs, HashMap<String, Integer> dp){
        String key = needs.toString();
        if(dp.containsKey(key)){
            return dp.get(key);
        }
        int min = calc(needs, price);
        for(int i = 0; i<special.size(); i++){
            List<Integer> offer = special.get(i);
            int n = needs.size();
            boolean offerValid = true;
            for(int j = 0; j< n; j++){
                if(needs.get(j)<offer.get(j)){
                    offerValid = false;
                    break;
                }
            }
            if(offerValid){
                for(int j = 0; j< n; j++){
                    needs.set(j, needs.get(j) - offer.get(j));
                }
            
                min = Math.min(min,offer.get(n)+backtracking(price, special,needs,dp));

                for(int j = 0; j< n; j++){
                    needs.set(j, needs.get(j) + offer.get(j));
                }
            }
        }
        dp.put(key,min);
        return min;
    }
    int calc(List<Integer> needs, List<Integer> price){
        int res = 0;
        int n = needs.size();
        for(int i = 0; i<n; i++){
            res+=needs.get(i)*price.get(i);
        }
        return res;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(s⋅n⋅m^n), where s is the length of special, n is the length of price and needs, and m is the number of unique values price[i] and needs[i] can take on.

* **Space Complexity**: O(m^n), where mmm is the number of unique values price[i] and needs[i] can take on, and nnn is the length of price and needs.