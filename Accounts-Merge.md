## Problem Highlights

* 🔗 **Leetcode Link:** [Accounts Merge](https://leetcode.com/problems/accounts-merge)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Graph, DFS, Adjacency List
* 🗒️ **Similar Questions**: [Redundant Connection](https://leetcode.com/problems/redundant-connection/), [Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii/), [Maximum Employees to Be Invited to a Meeting](https://leetcode.com/problems/maximum-employees-to-be-invited-to-a-meeting/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How might we handle accounts with the same first name?
  -  If two accounts have the same name but do not share any common emails, we can assume that there are two accounts with the same name. Similar in the example, we have two accounts with the name John but are represented by two separate instances in the output.

    
- Will names be case sensitive?
  - No. You can assume that there will not be two accounts with a similar email but different names as in "John" or "john"

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

EDGE CASE
Input: accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co"]]
Output: 
[["Gabe","Gabe0@m.co","Gabe3@m.co"]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:

- DFS/BFS: 
    - We can use a DFS on each account in accounts list and look up the emails accounts map to tell us which accounts are linked to that particular account via common emails. This will make sure we visit each account only once. This is a recursive process and we should collect all the emails that we encounter along the way. Lastly, it will allow us to sort the collected emails and add it to final results. 
- Union Find: 
    - Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
- Adjacency List: 
    - We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: 
    - We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: 
    - We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will create a adjacency list and find the common emails.  Do a DFS on each account in accounts list and look up to tell us which accounts are linked to that particular account via common emails.

```markdown
PYTHON
1. Create adjacency list list of email to emails
2. Create primaryEmail to name dictionary
3. Run DFS call to join associated emails in adjacency list under a single name 
    a. Create result to store results and visited to prevent email cycles.
    b. Define DFS call to join all related emails under allRelatedEmails.
    c. Make DFS Call on every primary email and collect all related emails
    d. If primary email had not been seen then we should get allRelatedEmail under one name for storage
4. Return results
```
```markdown
JAVA
1. Build a graph of an adjacency list of emails. 
2. Every email should have an edge to the connected email (including itself). From this, we can maintain a list of emails to account name list. 
3. Next, do a DFS for the unique email (using a hashset 'visited') to fill the emails for the given account name. 
4. Then, we can add the account name to the email address. 
5. Add the resultant account to end result.
```

⚠️ **Common Mistakes**

* How is this a graph problem? 
    * We can apply a graph data structure where we build a map that maps an email to a list of accounts, which can be used to track which email is linked to which account. Emails can be represented as nodes, and an edge between nodes will signify that they belong to the same person. Then we can add an edge between the two connected components, effectively merging them into one connected component. This is essentially our graph. For example:

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

* The underlying requirement is to group emails belonging to same person together. In other words: find the connected components. A slow solution would involve traversing each row, checking with other rows for connection. The runtime is n^2, where n = row size. A bottleneck would be repeatedly visiting rows. This solution is not optimal. 
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        # Create adjacency list of email to emails
        emailToEmails = defaultdict(set)

        # Create email to name dictionary
        primaryEmailToName = dict()

        for account in accounts:
            # Fill in primaryEmailToName Dictionary
            name = account[0]
            primaryEmail = account[1]
            primaryEmailToName[primaryEmail] = name

            # Fill in emailToEmails Adjacency List
            for email in account[1:]:
                emailToEmails[primaryEmail].add(email)
                emailToEmails[email].add(primaryEmail)
            

        # Run DFS call to join associated emails in Adjacency List under a single name 

        # Create result to store results and visited to prevent email cycles.
        result = []
        visited = set()

        # Define DFS call to join all related emails under allRelatedEmails.
        def dfs(email, allRelatedEmails):
            if email in visited:
                return
            visited.add(email)
            allRelatedEmails.add(email)
            relatedEmails = emailToEmails[email]
            for relatedEmail in relatedEmails:
                dfs(relatedEmail, allRelatedEmails)

        # Make DFS Call on every primary email and collect all related emails.
        for primaryEmail, name in primaryEmailToName.items():
            allRelatedEmails = set()
            dfs(primaryEmail, allRelatedEmails)
            
            # If primary email had not been seen then we should get allRelatedEmail under one name for storage
            if allRelatedEmails:
                result.append([name] + sorted(allRelatedEmails))
        
        # Return results
        return result
```
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
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of accounts with duplicates.
Assume `K` represents the maximum length of an account.

* **Time Complexity**: O(NK log NK) In the worst case, all the emails will end up belonging to a single person. The total number of emails will be N*K, and we need to sort these emails which takes log(N*K). DFS traversal will take N*K operations as no email will be traversed more than once.
* **Space Complexity**: O(N * K) Building the adjacency list will take O(NK) space. In the end, visited hashset will contain all of the emails, so it will use O(NK) space. Also, the recursive call stack for DFS will use O(NK) space in the worst case.

