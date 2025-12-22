# Day 17

### [Problem 1143: Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/description/)
Hints:
- Since subsequence is about order, can skip characters that not match, we have to use dp
- where it is 2d dp bottom up: text1 is row and text2 is column
- rememeber to assign extra row and column to 0 - len row = text1 + 1  and len col = text2 + 1
- if they are match --> move diagnal --> dp[i][j] = 1 + dp[i+1][j+1]
- else move right or bottom --> dp[i][j] = max(dp[i+1][j] or dp[i][j+1])

```
def longestCommonSubsequence(self, text1: str, text2: str) -> int:
    dp = [[0 for i in range(len(text2) + 1)] for j in range(len(text1) + 1)]    
    for i in range(len(text1) - 1, -1, -1):
        for j in range(len(text2) - 1, -1, -1):
            if text1[i] == text2[j]:
                dp[i][j] = 1 + dp[i + 1][j + 1]
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j+1])

    return dp[0][0]
```

