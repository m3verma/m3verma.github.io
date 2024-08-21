---
layout: default
---

# Plus One

You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

### Constraints

1 <= digits.length <= 100<br>
0 <= digits[i] <= 9<br>
digits does not contain any leading 0's.

### Example 1

**Input:** digits = [1,2,3]<br>
**Output:** [1,2,4]<br>
**Explanation:** The array represents the integer 123. Incrementing by one gives 123 + 1 = 124. Thus, the result should be [1,2,4].

### Example 2

**Input:** digits = [9]<br>
**Output:** [1,0]<br>
**Explanation:** The array represents the integer 9. Incrementing by one gives 9 + 1 = 10. Thus, the result should be [1,0].

### Example 3

**Input:** digits = [4,3,2,1]<br>
**Output:** [4,3,2,2]<br>
**Explanation:** The array represents the integer 4321. Incrementing by one gives 4321 + 1 = 4322. Thus, the result should be [4,3,2,2].


## Solution

First we increment the first digit (last element) by 1, if it becomes 10 we make it 0 ans add 1 to the second digit.. until the last digit (first element), if it becoms 10 we make it 1 and push_back a leading zero.

Since we have set a baseline lets try to code it :

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        carry=0
        for i in range(len(digits)-1, -1, -1):
            digits[i]=digits[i]+1
            if digits[i]!=10:
                carry=0
                break
            else:
                digits[i] = 0
                carry=1
        if carry==1:
            return [1]+digits
        else:
            return digits
```
