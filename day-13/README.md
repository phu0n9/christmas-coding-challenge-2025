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

### [Problem 55: Jump Game](https://leetcode.com/problems/jump-game/description/)
Hints:
- it can reach maximum of that position or 1 to nums[i], it does not care about how it get there but only care about the max to reach there so we can use greedy approach for this
- if current i > max_reach --> return False 
- keep track of max reach = max (max reach and nums[i] + i) --> at that index position how far it can go with the maximum distance
- if max reach >= len(nums) -1 --> return True

```
def canJump(self, nums: List[int]) -> bool:
    max_reach = 0
    for i in range(len(nums)):
        if i > max_reach:
            return False
        max_reach = max(max_reach, nums[i] + i)
        if max_reach >= len(nums) - 1:
            return True
    return True
```

