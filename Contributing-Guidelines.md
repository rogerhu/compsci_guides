Topic guides take a few different forms but generally speaking each guide is of one of these types:

 * **Topic introduction and cheatsheet** - Broad overview of a key topic
   - e.g Data Structures (Linked List) or Algorithms (Dynamic Programming)
   - Generally introductory, description of the topic, time/space complexity notes
   - Links to the pattern guides, includes examples of common problems
   - Links to videos or other resources for further reading
 * **Algorithm Pattern Breakdowns** - Use case guides breaking down a particular algorithm pattern
   - e.g Manipulating a LinkedList, 2-Pointer tracking
   - Describe the pattern and when it's generally used, example of problems where that is used
   - Example of one or two problems and solutions leveraging this pattern
   - Specific solution example (e.g. Length of the longest substring without repeating characters)

Please see the [section outlines below](#guide-section-outlines) for a much more detailed overview of each guide section.

## General Guidelines

### Best practices for the guides (👍):

 * Information hierarchy and order is important. Content near the top of the guide should be maximally important. 
 * Use section headings and breaks to help create a clear and consistent structure for the guide
 * Focus on the 80% most valuable cases. Avoid tangents or "20%" information except at the bottom of the guide. 
 * Leverage progressively revealed code snippets to simulate "working through solutions" by showing stages as a final solution is completed (with an explanation between stages) to help create an "aha" moment.
 * Leverage code comments that clearly highlight key elements of the algorithm or data structure code.
 * Try to make any introductory examples used relatable and accessible to as wide an audience as possible. 

### Avoid these pitfalls (⚠️):

 * Avoid using technical vocabulary (jargon) unless that vocabulary is defined explicitly in the guide itself, parent topic overview, or a foundational guide on the system.
 * Avoid language about the algorithm or data structure being "simple" or "easy". Often anything starts to feel easy with enough practice. Don't assume it is easy for everyone when they first read about it. 
 * Avoid multiple long paragraphs of text that don't have direct value to the reader. Keep the guides concise with "high informational content". In other words, avoid long exposition unless absolutely necessary. 

## Guide Section Outlines

### Data Structure Topic Intro Sections

e.g Linked List, Binary Trees

1. **Executive Summary** - Paragraph or two that provides as much an overall summary as possible. The specifics of the data structure, why and when this structure, the tl;dr of pros and cons of using this structure.
2. **Introduction** - Setup more context for this data structure including a visual example or two of how this structure is illustrated, small code snippets representing the structure
3. **Time/Space Complexity** - Table or description of the common O(n) for this structure on various operations
4. **Pros and Cons** - List of the advantages and disadvantages of this structure in more detail
5. **Glossary** - List of common terminology (should cover most anything used in the page or patterns)
6. **Basic Operations** - Code and descriptions for basic operations (too simple to be a full pattern)
7. **Patterns List** - List of links to the common memorized or "derived" algorithm patterns for this data structure
8. **References** - List of external links for additional reading on this structure

### Algorithm Topic Intro Sections

e.g Dynamic Programming

1. **Executive Summary** - Paragraph or two that provides as much a content-rich overall summary as possible. The specifics of the algorithm, why and when this algorithm is used, the tl;dr of pros and cons of using this structure.
2. **Introduction** - Setup more context for this algorithm including a visual example or two of how this is illustrated
3. **Time/Space Complexity** - Table or description of the common O(n) for this algorithm for major variations
4. **Glossary** - List of common terminology (should cover most anything used in the page or patterns)
5. **Basic Elements** - Code and descriptions for major elements of this algorithm (i.e Memoization)
6. **Patterns List** - List of links to the common memorized or "derived" patterns/techniques for this algorithm
7. **References** - List of external links for additional reading on this algorithm

### Algorithm Specific Pattern Breakdown Guide

e.g 2-Pointer Tracking for Linked List

1. **Executive Summary** - Paragraph or two that provides as much a content-rich overall summary as possible. The specifics of the pattern, why and when this pattern is used.
2. **Introduction** - Setup more context for this pattern, the specifics of the mechanics (illustrated with visually / code / description, whatever is easiest)
3. **Pose Canonical Problem** - Pick a canonical problem that can be solved primarily applying this pattern or technique. Pose the problem and setup context for why this pattern is needed (e.g explain why alternative approach would be insufficient or less efficient)
4. **Solve Canonical Problem** - In a step-by-step manner, break down the problem and solve this problem by leveraging the pattern described. Do this in a manner that gradually reveals the solution over 2-4 solution iterations to make this easier to pick-up.
5. (If needed) **Pose 2nd problem** - When needed, if the pattern has multiple distinct variations, to drive the point home, post, breakdown and solve a second problem demonstrating the pattern in a new context. 
6. **Problems List** - List 2-5 other seemingly distinct problems (maybe title with link to LeetCode) in which this pattern (or small variation) would be a principal component of the solution.
7. **References** - List of external links for additional reading on this algorithm