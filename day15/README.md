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

