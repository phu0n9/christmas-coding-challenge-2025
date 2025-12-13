# Day 13:
- 8 Problems

### [Problem 53: Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)
Hints:
- Kadane'algorithm
- this one pattern is whether should i choose this current number or should i add this number to the max
- 2 numbers, starting from nums[0]
    - one is temp to check max of including the current number or only including the current number  
    - one is res to update the max

```
def maxSubArray(self, nums: List[int]) -> int:
    temp = nums[0]
    res = nums[0]
    for i in range(1, len(nums)):
        temp = max(nums[i], nums[i] + temp)
        res = max(res, temp)
    return res
```