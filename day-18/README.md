# Day 18

### [Problem 518: Coin Change II](https://leetcode.com/problems/coin-change-ii/description/)
Hints:
- Use DP 2d for it
- Grid :
    - Amount: 
    -         5 4 3 2 1 0
    - Coins:  
    -         1
    -         2
    -         5
- Do from bottom up and set the first value of amount 0 is 1 --> dp[i][0] = 1 where i is the coins

```
def change(self, amount: int, coins: List[int]) -> int:
    dp = [[0] * (amount + 1) for j in range(len(coins) + 1)]
    for i in range(len(coins) + 1):
        dp[i][0] = 1
    
    for i in range(len(coins) - 1, -1, -1):
        for j in range(1, amount + 1):
            dp[i][j] = dp[i+1][j]
            if j >= coins[i]:
                dp[i][j] += dp[i][j - coins[i]]

    return dp[0][amount]
```