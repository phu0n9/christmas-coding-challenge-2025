# Day 22

### [Problem 139: Word Break](https://leetcode.com/problems/word-break/description/)
Hints:
- we should use the function like this: s.startswith(word, current index) --> s = "leetcode" and wordDict = ["leet", "code"]
    - then s.startswith("leet", 0) --> True
    - then s.startswith("code", 4) --> True
    - then s.startswith("code", 0) --> False
- use dfs: iterate through wordDict: if the s.startswith(word, current index) is True then we need to loop if the next index of the word like current index + len(word) --> next word break is in the wordDict or not --> recursive here --> if it is True then return True, outside of the wordDict loop return False if it does not find anything

```
def wordBreak(self, s: str, wordDict: List[str]) -> bool:
    dp = {}
    def dfs(left):
        if left in dp:
            return dp[left]
        if left == len(s):
            return True
        
        for i in range(len(wordDict)):
            word = wordDict[i]
            if s.startswith(word, left):
                dp[left] = dfs(left + len(word))
                if dp[left]:
                    return True
        return False
    return dfs(0)
```

### [Problem 416: Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/description/)
Hints:
- if split into subsets and sum of the elements in both subsets is equal --> meaning the total sum of array should be divisible by 2
- So we need to have sum(nums) % 2 --> False as base case
- use dfs and then add memorization later, we have 2 arguments current index and target = sum(nums) // 2
    - if current index >= len nums: check if target = 0 or not
    - if target < 0 --> return False
    - then we do recursive where we either choose target by the current target with the current index + 1 OR we choose target = target - nums[current index] and along with currrent index + 1

```
def canPartition(self, nums: List[int]) -> bool:
    if sum(nums) % 2:
        return False
    memo = [[-1] * ((sum(nums) // 2) + 1) for _ in range(len(nums) + 1)]

    def dfs(left, target):
        if target == 0:
            return True
        if memo[left][target] != -1:
            return memo[left][target]
        
        if left >= len(nums) or target < 0:
            return False
        
        memo[left][target] = dfs(left + 1, target) or dfs(left + 1, target - nums[left])
        return memo[left][target]
    return dfs(0, sum(nums) // 2)
```

