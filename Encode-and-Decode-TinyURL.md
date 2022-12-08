## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/encode-and-decode-tinyurl/>
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 10 to 13 mins
* 🛠️ **Topics**: Hash Table
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What is the time complexity limited by?
 - The range of URLs that can be decoded is limited by the range of int.
- Is the length of the URL shorter than the incoming longURL?
 - The length of the URL isn't necessarily shorter than the incoming longURL. It is only dependent on the relative order in which the URLs are encoded.

Run through a set of example cases:

```markdown
Input: url = "https://leetcode.com/problems/design-tinyurl"
Output: "https://leetcode.com/problems/design-tinyurl"

Explanation:
Solution obj = new Solution();
string tiny = obj.encode(url); // returns the encoded tiny url.
string ans = obj.decode(tiny); // returns the original url after deconding it.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use the key-value pair concept to store the relationship between shortURL and originalURL.

```markdown
1. Encodes a URL to a shortened URL.
a. if encoded url already exists for a given longUrl (don't want duplicates for same url)
b. generate a random code from alphanumeric string
2. Decodes a shortened URL to its original URL.
```

⚠️ **Common Mistakes**

* Some people may try to approach this problem initially with some brute force O(N^2) strategy. However, this approach doesn’t work. Try to urge students to avoid usual brute force tactics. Instead, urge them to use known data structures to solve this problem.
* If excessively large number of URLs have to be encoded, after the range of int is exceeded, integer overflow could lead to overwriting the previous URLs' encodings, leading to the performance degradation.
* If a hashmap is not used, one problem with using a simple counter is that it is very easy to predict the next code generated, since the pattern can be detected by generating a few encoded URLs.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Codec:
    
    def __init__(self):
        
        self.url_table = dict()
    
    def encode(self, longUrl: str) -> str:
        """Encodes a URL to a shortened URL.
        """

        # generate url id by native hash() function
        url_id = hash(longUrl)
        header = 'http://tinyurl.com/'
        
        # generate shour url
        short_url = header + str(url_id)

		# update key-value pair in dictionary, url_table
        self.url_table[url_id] = longUrl
        
        return short_url
        

    def decode(self, shortUrl: str) -> str:
        """Decodes a shortened URL to its original URL.
        """
        
        # paring the url id after 'http://tinyurl.com/'
        url_id = int(shortUrl[19:])
        
        # lookup original url by url_id in dictionary
        return self.url_table[url_id]
```
```java
public class Codec {
    private HashMap<Long, String> db = null;
    private long id = 0;
    private char[] dict = null;
    private int BASE = 0;

    public Codec() {
        db = new HashMap<>();
        dict = new char[]{'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n',
 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 
'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z',
 '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
        BASE = dict.length;
    }

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        long index = insertUrl(longUrl);
        StringBuilder sb = new StringBuilder();
        while (index > 0) {
            sb.insert(0, dict[(int) (index % BASE)]);
            index /= BASE;
        }
        return sb.toString();
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        long index = 0;
        for (char c : shortUrl.toCharArray()) {
            if ('a' <= c && c <= 'z')
                index = index * 62 + c - 'a';
            if ('A' <= c && c <= 'Z')
                index = index * 62 + c - 'A' + 26;
            if ('0' <= c && c <= '9')
                index = index * 62 + c - '0' + 52;
        }

        return readUrl(index);
    }

    private String readUrl(long index) {
        if (db.containsKey(index))
            return db.get(index);
        return "";
    }

    private long insertUrl(String longUrl) {
        id++;
        db.put(id, longUrl);
        return id;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**:
In encode, find complexity is O(1). The possibility of it hitting a code which is already existing is really less, so you can take the time complexity to map shortUrl and longUrl as O(1).
* **Space Complexity**: O(n), directly proportional to the amount of URLs you input.