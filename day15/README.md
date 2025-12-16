# Day 15

### [Problem 5: Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/description/)
Hints:
- There are only two possibilities:
    - One character → odd length (e.g. "racecar")
    - Between two characters → even length (e.g. "abba")
- for every index i we need to spread left and right index until they are not the same and left >= 0 and right < len string
- find two cases for odd and even length
- if one of those cases have more length than the current result, update it

```
def longestPalindrome(self, s: str) -> str:
    if not s:
        return ""
    res = ""
    def expand(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return s[left+1 : right]
    
    for i in range(len(s)):
        p1 = expand(i, i)
        p2 = expand(i, i+1)
        if len(p1) > len(res):
            res = p1
        if len(res) < len(p2):
            res = p2
    return res
```

### [Problem 647: Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/description/)
Hints:
- Like problem 5 but just adding the count inside
- pandlindrome can have odd and even length and we need to spread left and right
- For odd: in every index: left = i, right = i
- For even: in every index: left = i, right = i + 1

```
def countSubstrings(self, s: str) -> int:
    def expand(left, right):
        count = 0
        while left >= 0 and right < len(s) and s[left] == s[right]:
            count += 1
            left -= 1
            right += 1
        return count
    res = 0
    for i in range(len(s)):
        res += expand(i, i)
        res += expand(i, i + 1)
    return res
```

### [Problem 152: Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description/)
Hints:
- It is not Kadane's algorithm because: when negative number appear and later it also has negative number then it will become positibe and can become max number
- So we need to keep track of previous min and previous max because previous min can become previous max
- it can be curr_max = max (num or prev_min * num or prev_max * num) and the same with curr_min = min (num or prev_min * num or prev_max * num)
- Later assign curr_min and curr_max to prev_min and prev_max respectively
- update max result

```
def maxProduct(self, nums: List[int]) -> int:
    prev_min = nums[0]
    prev_max = nums[0]
    res = nums[0]
    for num in nums[1:]:
        candidates = (num, prev_min * num, prev_max * num)
        curr_min = min(candidates)
        curr_max = max(candidates)
        prev_max = curr_max
        prev_min = curr_min
        res = max(prev_max, res)

    return res
```

### [Problem 91: Decode Ways](https://leetcode.com/problems/decode-ways/description/)
Hints:
- There are two ways of decoding: single digit and two digits
- base case is that empty string has exact one way to decode dp[0] = 1 and first character would also have one way to decode: dp[1] = 1 if first character is not "0"
- And then single digit would depend on dp[i-1] --> curr = dp[i-1] → number of ways to decode up to previous character
- two digits would depend on dp[i-2] --> prev = dp[i-2] → number of ways to decode up to two characters back
- so:
    - prev -> dp[i-2]: Two digits → ways from two characters back
    - curr -> dp[i-1]: Single digit → ways from last character
    - temp -> dp[i]: Add them → total ways up to this index
- then slide the window

```
def numDecodings(self, s: str) -> int:
    if not s or s[0] == '0':
        return 0
    prev, curr = 1, 1
    for i in range(1, len(s)):
        temp = 0
        if s[i] != '0':
            temp += curr
        if 10 <= int(s[i-1: i+1]) <= 26:
            temp += prev
        prev, curr = curr, temp
    return curr
```

### [Problem 322: Coin Change](https://leetcode.com/problems/coin-change/description/)
Hints:
- Think of the problem as climbing a ladder:
    - Each step = 1 unit of amount.
    - Each coin = a possible “jump” you can take.
- Define the subproblem:
    - dp[i] = minimum number of coins needed to make amount i.
    - Base case: dp[0] = 0 (0 coins to make amount 0).
- Build up from smaller amounts:
    - For each amount i, try every coin c that can fit (c <= i).
    - If you use c as the last coin, the remaining amount is i - c.
    - Total coins = dp[i - c] + 1.
- Keep the best solution:
- There may be multiple coins you can try.
- Use dp[i] = min(dp[i], dp[i - c] + 1) to always store the fewest coins.

Why it works:

DP leverages optimal substructure: the best solution for i depends only on the best solutions for smaller amounts.

By iterating from 1 → amount, all smaller subproblems are already solved.

- Edge case:
    If dp[amount] is still infinity at the end → it’s impossible to form that amount → return -1.

```
def coinChange(self, coins: List[int], amount: int) -> int:
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0

    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1
```