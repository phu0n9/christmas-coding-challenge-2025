# Day 21

### [Problem 66: Plus One](https://leetcode.com/problems/plus-one/description/)
Hints:
- decrement from right to left:
    - if the digits < 9 then just adding one and return the array
    - if it is 9 then set that current digit is 0
- return [1] + arr

```
def plusOne(self, digits: List[int]) -> List[int]:
    for i in range(len(digits) - 1, -1, -1):
        if digits[i] < 9:
            digits[i] += 1
            return digits
        digits[i] = 0
    
    return [1] + digits
```

### [Problem 50: Pow(x, n)](https://leetcode.com/problems/powx-n/description/)
Hints:
- if n < 0 --> reverse n to positive and x = 1 / x
- so if the n is even --> x ^ n = (x^2)^(n/2) --> the result is halved
- n is odd --> x ^ n = x ^ (n * (n - 1)) where n - 1 is even number so we need a condition to multiply the x when n is odd

```
def myPow(self, x: float, n: int) -> float:
    if n < 0:
        x = 1 / x
        n = -n
    
    res = 1.0
    while n > 0:
        if n % 2 == 1:
            res *= x
        x *= x
        n //= 2
    
    return res
```

### [Problem 300: Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/description/)
Hints:
- Let dp[i] be the length of the longest increasing subsequence that starts at index i.
- Each number alone is a subsequence → initialize dp[i] = 1.
- Decision at each index:
    - For nums[i], look at all numbers to its right (j > i).
    - If nums[i] < nums[j], we can place nums[i] in front of the subsequence starting at j.
    - Then the subsequence length starting at i becomes dp[j] + 1.
    - We take the maximum among all valid j to find the best sequence starting at i.
- The LIS can start anywhere, so the answer is max(dp).

```
def lengthOfLIS(self, nums: List[int]) -> int:
    n = len(nums)
    dp = [1] * len(nums)
    for i in range(len(nums) - 1, -1 , -1):
        for j in range(i+1, len(nums)):
            if nums[i] < nums[j]:
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)
```
