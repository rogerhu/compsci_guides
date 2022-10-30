## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/accounts-merge](https://leetcode.com/problems/accounts-merge)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: [Redundant Connection](https://leetcode.com/problems/redundant-connection/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we identify an account?
  - We give each account an ID, based on the index of it within the list of accounts. For example: 
    
    ```
    [
      ["John", "johnsmith@mail.com", "john@mail.com"], # Account 0
      ["John", "johnnybravo@mail.com"], # Account 1
      ["John", "johnsmith@mail.com", "john_smith@mail.com"],  # Account 2
      ["Jane", "jane@mail.com"] # Account 3
    ]
    ```
    
- Can one person have multiple accounts? 
  - One person is allowed to have multiple accounts, but each email can only belong to one person.
    
- Why do need to list out all the emails that belong to a specific person? 
  - This is done so that every time we find two accounts with an email in common, we will merge the two accounts into one.
    
- What do you mean by “merging” accounts? 
  - We have a set of elements (emails) that are connected (belonging to the same user). We can consider this as our input on a graph. Converting the input into a graph is what is meant by “merging” the accounts.
    
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
    
How is this a graph problem? We can apply a graph data structure where we build a map that maps an email to a list of accounts, which can be used to track which email is linked to which account. Emails can be represented as nodes, and an edge between nodes will signify that they belong to the same person. Then we can add an edge between the two connected components, effectively merging them into one connected component. This is essentially our graph. For example:
    
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
    
- DFS: We can use a DFS on each account in accounts list and look up the emails accounts map to tell us which accounts are linked to that particular account via common emails. This will make sure we visit each account only once. This is a recursive process and we should collect all the emails that we encounter along the way. Lastly, it will allow us to sort the collected emails and add it to final results. 
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.


## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Do a DFS on each account in accounts list and look up to tell us which accounts are linked to that particular account via common emails.
    
Build a graph with an adjacency list of emails. Every email should have an edge to the connected email (including itself). From this, we can maintain a list of emails to account name list. Next, do a DFS for the unique email (using a hashset 'visited') to fill the emails for the given account name. Then, we can add the account name to the email address. Add the resultant account to end result.
    

⚠️ **Common Mistakes**

* The underlying requirement is to group emails belonging to same person together. In other words: find the connected components. A slow solution would involve traversing each row, checking with other rows for connection. The runtime is n^2, where n = row size. A bottleneck would be repeatedly visiting rows. This solution is not optimal. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
class Solution {
    HashSet<String> visited = new HashSet<>();
    Map<String, List<String>> adjacent = new HashMap<String, List<String>>();
    
    private void DFS(List<String> mergedAccount, String email) {
        visited.add(email);
        // Add the email vector that contains the current component's emails
        mergedAccount.add(email);
        
        if (!adjacent.containsKey(email)) {
            return;
        }
        
        for (String neighbor : adjacent.get(email)) {
            if (!visited.contains(neighbor)) {
                DFS(mergedAccount, neighbor);
            }
        }
    }
    
    public List<List<String>> accountsMerge(List<List<String>> accountList) {
        int accountListSize = accountList.size();
        
        for (List<String> account : accountList) {
            int accountSize = account.size();
            
            // Building adjacency list
            // Adding edge between first email to all other emails in the account
            String accountFirstEmail = account.get(1);
            for (int j = 2; j < accountSize; j++) {
                String accountEmail = account.get(j);
                
                if (!adjacent.containsKey(accountFirstEmail)) {
                    adjacent.put(accountFirstEmail, new ArrayList<String>());
                }
                adjacent.get(accountFirstEmail).add(accountEmail);
                
                if (!adjacent.containsKey(accountEmail)) {
                    adjacent.put(accountEmail, new ArrayList<String>());
                }
                adjacent.get(accountEmail).add(accountFirstEmail);
            }
        }
        
        // Traverse over all th accounts to store components
        List<List<String>> mergedAccounts = new ArrayList<>();
        for (List<String> account : accountList) {
            String accountName = account.get(0);
            String accountFirstEmail = account.get(1);
            
            // If email is visited, then it's a part of different component
            // Hence perform DFS only if email is not visited yet
            if (!visited.contains(accountFirstEmail)) {
                List<String> mergedAccount = new ArrayList<>();
                // Adding account name at the 0th index
                mergedAccount.add(accountName);
                
                DFS(mergedAccount, accountFirstEmail);
                Collections.sort(mergedAccount.subList(1, mergedAccount.size())); 
                mergedAccounts.add(mergedAccount);
            }
        }
        
        return mergedAccounts;
    }
}
```
    
```python
    class Solution(object):
        def accountsMerge(self, accounts):
            from collections import defaultdict
            visited_accounts = [False] * len(accounts)

            # add the emails that contains the current component's emails
            emails_accounts_map = defaultdict(list)
            res = []
          
            for i, account in enumerate(accounts):
                for j in range(1, len(account)):
                    email = account[j]
                    emails_accounts_map[email].append(i)
          

            # if email is visited, then it's a part of different component
            # then perform DFS only if email is not visited yet
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
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(V + E)`, where `V` is the number accounts (can contain duplicates) and `E` is the number of accounts (without any duplicates)
<br>
Space Complexity: `O(V + E)`, where `V` is the number accounts (can contain duplicates) and `E` is the number of accounts (without any duplicates)