# Day 19

### [Problem 494: Target Sum](https://leetcode.com/problems/target-sum/)
Hints:
- We can use memorization method by adding dp dictionary and backtrack method

```
def findTargetSumWays(self, nums: List[int], target: int) -> int:
    dp = {}
    def backtrack(i, curr_sum):
        if (i, curr_sum) in dp:
            return dp[(i, curr_sum)]
        if i == len(nums):
            return 1 if target == curr_sum else 0
        
        dp[(i, curr_sum)] = (backtrack(i + 1, curr_sum - nums[i]) + backtrack(i + 1, curr_sum + nums[i]))
        return dp[(i, curr_sum)]
    
    return backtrack(0, 0)
```

- Bottom up method can do the same but we use 2d array where we store dp[i][j] where i is the index in num array and j is the curr_sum, we save the dp[i][j] to be a hashmap --> we can have -5 and 5 or -3 or 3 --> these can be the target depending how we increment or decrement
- we have to set the first number of curr_sum is 0 (1 way for 0 numbers to get 0 sum)

```
def findTargetSumWays(self, nums: List[int], target: int) -> int:
    dp = [defaultdict(int) for _ in range(len(nums) + 1)]
    dp[0][0] = 1
    
    for i in range(len(nums)):
        for curr_sum, count in dp[i].items():
            dp[i+1][curr_sum + nums[i]] += count
            dp[i+1][curr_sum - nums[i]] += count
    
    return dp[len(nums)][target]
```

### [Problem 97: Interleaving String](https://leetcode.com/problems/interleaving-string/description/)
Hints:
- We can do this by memorization:
```
def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False

        dp = {}
        def backtrack(i, j):
            if (i, j) in dp:
                return dp[(i, j)]

            k = i + j

            if k == len(s3):
                return i == len(s1) and j == len(s2)

            ans = False
            if i < len(s1) and s1[i] == s3[k]:
                ans = ans or backtrack(i+1, j)

            if j < len(s2) and s2[j] == s3[k]:
                ans = ans or backtrack(i, j+1)

            dp[(i, j)] = ans
            return ans

        return backtrack(0, 0)
```

- Or we can solve this bottom up (length of two string to create 2d array) and remember if we are at index i then it is either dp[i+1][j] is True (right) or dp[i][j+1] is True --> case like s3 = a and we can choose s1 = a or s2 = a if s1 at this position is the same as s3 and the same with s2
- Base case is dp[len(s1)][len(s2)] would be True because 2 strings can make it up to s3
```
def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
    if len(s1) + len(s2) != len(s3):
        return False
    dp = [[False] * (len(s2) + 1) for _ in range(len(s1) + 1)]
    dp[len(s1)][len(s2)] = True
    
    for i in range(len(s1), - 1, -1):
        for j in range(len(s2), -1, -1):
            if i < len(s1) and s1[i] == s3[i + j] and dp[i + 1][j]:
                dp[i][j] = True

            if j < len(s2) and s2[j] == s3[i + j] and dp[i][j + 1]:
                dp[i][j] = True
    
    return dp[0][0]
```

### [Problem 134: Gas Station](https://leetcode.com/problems/gas-station/description/)
Hints:
- sum of gas < sum of cost --> it is not possible --> return -1
- Find the difference between tank += gas[i] - cost[i] then if it is negative then we reset the tank and increment the start by 1
- gas = [1,2,3,4,5], cost = [3,4,5,1,2] --> diff = [-2, -2, -2, 3, 3]
- so if we start at index 4 (gas = 4) we will have 3 in tank and we go next, we will have 6 and we go circular, we have 4, 2, and 0 and then back at index 4

```
def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
    if sum(gas) < sum(cost):
        return -1
    
    tank = 0
    start = 0
    for i in range(len(gas)):
        tank += gas[i] - cost[i]
        if tank < 0:
            tank = 0
            start = i + 1
    return start
```

### [Problem 136: Single Number](https://leetcode.com/problems/single-number/description/)
Hints:
- Use bit wise operator OR

```
def singleNumber(self, nums: List[int]) -> int:
    res = 0
    for i in range(len(nums)):
        res ^= nums[i]
    return res
```