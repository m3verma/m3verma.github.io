---
layout: default
---

# Majority Element

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

### Constraints

n == nums.length<br>
1 <= n <= 5 * 10<sup>4</sup><br>
-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup>

### Example 1

**Input:** nums = [3,2,3]<br>
**Output:** 3<br>

### Example 2

**Input:** nums = [2,2,1,1,1,2,2]<br>
**Output:** 2<br>


## Solution

The intuition behind the Moore's Voting Algorithm is based on the fact that if there is a majority element in an array, it will always remain in the lead, even after encountering other elements. The algorithm works on the basis of the assumption that the majority element occurs more than n/2 times in the array. This assumption guarantees that even if the count is reset to 0 by other elements, the majority element will eventually regain the lead.

Since we have set a baseline lets try to code it :

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        count=0
        digit=0
        for i in range(0, len(nums)):
            if count==0:
                digit=nums[i]
                count=count+1
            elif digit!=nums[i]:
                count=count-1
            else:
                count=count+1
        return digit
```



