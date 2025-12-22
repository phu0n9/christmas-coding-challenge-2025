# Day 14

### [Problem 70: Climbing Stairs](https://leetcode.com/problems/climbing-stairs/description/)
Hints:
- I use memorization method in which we use dfs to solve and store the previous number in hashmap

```
def climbStairs(self, n: int) -> int:
    memo = {}
    def dfs(n):
        if n < 0:
            return 0
        if n == 0:
            return 1
        if n in memo:
            return memo[n]
        memo[n] = dfs(n - 1) + dfs(n - 2)
        return memo[n]
    return dfs(n)
```

### [Problem 746: Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/description/)
Hints:
- it is like problem 70: Climbing stairs, the trick is that we should start at the top of the stairs and climb down
- so it becomes like problem 70: min(top - 1 step or top - 2 steps) + current cost at that position
- base if condition is current index <= 1 then we return that current cost at that index (since we can start at index 0 or 1)

```
def minCostClimbingStairs(self, cost: List[int]) -> int:
    memo = {}
    n = len(cost)
    def dfs(i):
        if i <= 1:
            return cost[i]
        
        if i in memo:
            return memo[i]
        memo[i] = cost[i] + min(dfs(i - 1), dfs(i - 2))
        return memo[i]
    return min(dfs(n - 1), dfs(n - 2))
```

### [Problem 198: House Robber](https://leetcode.com/problems/house-robber/description/)
Hints:
- trick is when you are at house[i] you can rob house[i - 1] or rob house[i-2] so the money can be max(house[i - 1], house[i - 2] + current money at house[i])

```
def rob(self, nums: List[int]) -> int:
    memo = {}
    def dfs(i):
        if i < 0:
            return 0
        if i in memo:
            return memo[i]
        memo[i] = max(nums[i] + dfs(i - 2), dfs(i - 1))
        return memo[i]
    return dfs(len(nums) - 1)
```

### [Problem 213: House Robber II](https://leetcode.com/problems/house-robber-ii/description/)
Hints:
- The same problem 198 but now we need to split two cases and find the max: excluding first house or excluding last house
- If doing memorization method, then use different hahsmap

```
def rob(self, nums: List[int]) -> int:
    if len(nums) == 1:
        return nums[0]
    def dfs(n, arr, memo):
        if n < 0:
            return 0
        if n in memo:
            return memo[n]
        memo[n] = max(dfs(n - 1, arr, memo), arr[n] + dfs(n - 2, arr, memo))
        return memo[n]

    memo1 = {}
    money1 = dfs(len(nums) - 2, nums[:-1], memo1)

    memo2 = {}
    money2 = dfs(len(nums) - 2, nums[1:], memo2)
    return max(money1, money2)
```