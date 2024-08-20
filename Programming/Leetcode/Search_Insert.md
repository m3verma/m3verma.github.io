---
layout: default
---

# Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

### Constraints

1 <= nums.length <= 10<sup>4</sup> <br>
-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup> <br>
nums contains distinct values sorted in ascending order.<br>
-10<sup>4</sup> <= target <= 10<sup>4</sup> 

### Example 1

**Input:** nums = [1,3,5,6], target = 5<br>
**Output:** 2<br>

### Example 2

**Input:** nums = [1,3,5,6], target = 2<br>
**Output:** 1<br>

### Example 3

**Input:** nums = [1,3,5,6], target = 7<br>
**Output:** 4<br>


## Solution

To efficiently solve this problem, a binary search algorithm is employed. Binary search is a common method for searching in a sorted array because it divides the search space in half with each step, leading to an optimal time complexity.

Since we have set a baseline lets try to code it :

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        L=0
        R=len(nums)-1
        while L<=R:
            mid = int((L+R)/2)
            if nums[mid]==target:
                return mid
            if target>nums[mid]:
                L=mid+1
            else:
                R=mid-1
        return L
```
