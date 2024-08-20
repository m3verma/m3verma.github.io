---
layout: default
---

# Find the Index of the First Occurrence in a String

Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

### Constraints

1 <= haystack.length, needle.length <= 10<sup>4</sup> <br>
haystack and needle consist of only lowercase English characters.

### Example 1

**Input:** haystack = "sadbutsad", needle = "sad"<br>
**Output:** 0<br>
**Explanation:** "sad" occurs at index 0 and 6. The first occurrence is at index 0, so we return 0.

### Example 2

**Input:** haystack = "leetcode", needle = "leeto"<br>
**Output:** -1<br>
**Explanation:** "leeto" did not occur in "leetcode", so we return -1.


## Solution

Use an inbuilt python function.

Since we have set a baseline lets try to code it :

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        return haystack.find(needle)
```
