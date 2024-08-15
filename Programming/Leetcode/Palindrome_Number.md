---
layout: default
---

# Palindrome Number

Given an integer x, return true if x is a palindrome, and false otherwise.

### Constraints

-2<sup>31</sup> <= x <= 2<sup>31</sup> - 1<br>

### Example 1

**Input:** x = 121<br>
**Output:** true<br>
**Explanation:** 121 reads as 121 from left to right and from right to left.

### Example 2

**Input:** x = -121<br>
**Output:** false<br>
**Explanation:** From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.

### Example 3

**Input:** x = 10<br>
**Output:** false<br>
**Explanation:** Reads 01 from right to left. Therefore it is not a palindrome.

## Solution

First, the code checks if x is negative. If it is, then it cannot be a palindrome since the reverse of a negative number will not be equal to the original number. In such a case, the code returns False. If number is multi digit it reverses it and then checks if it is equal to original number or not.

Since we have set a baseline lets try to code it :

```c++
class Solution {
 public:
  bool isPalindrome(int x) {
    if (x < 0)
      return false;

    long reversed = 0;
    int y = x;

    while (y > 0) {
      reversed = reversed * 10 + y % 10;
      y /= 10;
    }

    return reversed == x;
  }
};
```

```python
class Solution:
  def isPalindrome(self, x: int) -> bool:
    if x < 0:
      return False

    rev = 0
    y = x

    while y:
      rev = rev * 10 + y % 10
      y //= 10

    return rev == x
```
