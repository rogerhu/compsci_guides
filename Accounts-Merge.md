## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/accounts-merge](https://leetcode.com/problems/accounts-merge)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: TBD

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we identify an account?
    We give each account an ID, based on the index of it within the list of accounts. For example: 
    
    ```
    [
      ["John", "johnsmith@mail.com", "john@mail.com"], # Account 0
      ["John", "johnnybravo@mail.com"], # Account 1
      ["John", "johnsmith@mail.com", "john_smith@mail.com"],  # Account 2
      ["Jane", "jane@mail.com"] # Account 3
    ]
    ```
    
- Can one person have multiple accounts? One person is allowed to have multiple accounts, but each email can only belong to one person.
    
- Why do need to list out all the emails that belong to a specific person? This is done so that every time we find two accounts with an email in common, we will merge the two accounts into one.
    
- What do you mean by “merging” accounts? We have a set of elements (emails) that are connected (belonging to the same user). We can consider this as our input on a graph. Converting the input into a graph is what is meant by “merging” the accounts.
    
    ```markdown
    HAPPY CASE
    Input: accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
    Output: [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
    
    Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
    Output: [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]
    ```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
    How is this a graph problem?
    
    We can apply a graph data structure where we build a map that maps an email to a list of accounts, which can be used to track which email is linked to which account. Emails can be represented as nodes, and an edge between nodes will signify that they belong to the same person. Then we can add an edge between the two connected components, effectively merging them into one connected component. This is essentially our graph. For example:
    
    ```
    # emails_accounts_map of email to account ID
    {
      "johnsmith@mail.com": [0, 2],
      "john123@mail.com": [0],
      "john@mail.com": [1],
      "john_smith@mail.com": [2],
      "jane@mail.com": [3]
    }
    ```
    
    We can use a DFS on each account in accounts list and look up the emails accounts map
    to tell us which accounts are linked to that particular account via common emails. This will make sure we visit each account only once. This is a recursive process and we should collect all the emails that we encounter along the way. Lastly, it will allow us to sort the collected emails and add it to final results. 
    
## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
    Build a graph with an adjacency list of emails. Every email should have an edge to the connected email (including itself). From this, we can maintain a list of emails to account name list. Next, do a DFS for the unique email (using a hashset 'visited') to fill the emails for the given account name. Then, we can add the account name to the email address. Add the resultant account to end result.
    

⚠️ **Common Mistakes**

* 

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        public List<List<String>> accountsMerge(List<List<String>> accounts) {
            Map<String, List<Integer>> names = new HashMap<>(); // map email to names using indexes
            for (int i = 0; i < accounts.size(); i++) {
                List<String> data = accounts.get(i);
                for (int j = 1; j < data.size(); j++) {
                    String email = data.get(j);
                    List<Integer> list = names.get(email);
                    if (list == null) {
                        list = new ArrayList<Integer>();
                        names.put(email, list);
                    }
                    list.add(i);
                }
            }
            boolean[] visited = new boolean[accounts.size()];
            List<List<String>> res = new LinkedList<>();
            for (int i = 0; i < accounts.size(); i++) {
                Set<String> set = new TreeSet<String>();
                dfs(i, accounts, names, visited, set);
                if (!set.isEmpty()) {
                    List<String> list = new LinkedList<String>(set);
                    list.add(0, accounts.get(i).get(0));
                    res.add(list);
                }
            }
            return res;
        }
        private void dfs(int cur, List<List<String>> accounts, Map<String, List<Integer>> names, 
                         boolean[] visited, Set<String> set) {
            if (visited[cur]) {
                return;
            }
            visited[cur] = true;
            for (int i = 1; i < accounts.get(cur).size(); i++) {
                String email = accounts.get(cur).get(i);
                set.add(email);
                for (int index : names.get(email)) {
                    dfs(index, accounts, names, visited, set);
                }
            }
        }
    }
```
    
```python
    class Solution(object):
        def accountsMerge(self, accounts):
            from collections import defaultdict
            visited_accounts = [False] * len(accounts)
            emails_accounts_map = defaultdict(list)
            res = []
          
            for i, account in enumerate(accounts):
                for j in range(1, len(account)):
                    email = account[j]
                    emails_accounts_map[email].append(i)
          
            def dfs(i, emails):
                if visited_accounts[i]:
                    return
                visited_accounts[i] = True
                for j in range(1, len(accounts[i])):
                    email = accounts[i][j]
                    emails.add(email)
                    for neighbor in emails_accounts_map[email]:
                        dfs(neighbor, emails)
            
            for i, account in enumerate(accounts):
                if visited_accounts[i]:
                    continue
                name, emails = account[0], set()
                dfs(i, emails)
                res.append([name] + sorted(emails))
            return res
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(V + E)`, where `V` is the number accounts (can contain duplicates) and `E` is the number of accounts (without any duplicates)
<br>
Space Complexity: `O(V + E)`, where `V` is the number accounts (can contain duplicates) and `E` is the number of accounts (without any duplicates)