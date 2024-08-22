---
layout: default
---

# Number Complement

The complement of an integer is the integer you get when you flip all the 0's to 1's and all the 1's to 0's in its binary representation.

For example, The integer 5 is "101" in binary and its complement is "010" which is the integer 2.

Given an integer num, return its complement.

### Constraints

1 <= num < 2<sup>31</sup>

### Example 1

**Input:** nums = 5<br>
**Output:** 2<br>
**Explanation:** The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.

### Example 2

**Input:** nums = 1<br>
**Output:** 0<br>
**Explanation:** The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.


## Solution

When asked to find the complement of an integer, the idea is to flip every bit in its binary representation—changing 0s to 1s and 1s to 0s. For example, the complement of 5 (which is 101 in binary) is 010, which is 2 in decimal.


Since we have set a baseline lets try to code it :

```python
class Solution:
    def findComplement(self, num: int) -> int:
        digits=[]
        ans=0
        while num>0:
            digits.append(num%2)
            num=num//2
        for i in range(len(digits)):
            if digits[i]==0:
                digits[i]=1
            else:
                digits[i]=0
        for i in range(len(digits)-1, -1, -1):
            if digits[i]==1:
                ans=ans+2**i
        return ans
```



