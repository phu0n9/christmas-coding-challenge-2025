# Day 1

## [Problem 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/description/)

Hints:
- list occurrences in s1 with hash table 26 characters
- loop through hash table 26 characters s2 and add occurrences in s2
- if count1 == count2 —> true
- every time if current character current index- start index + 1 == len(string1)
    - count2 current character - 1
    - increase starting index


```
 def checkInclusion(self, s1: str, s2: str) -> bool:
        count1 = [0] * 26
        count2 = [0] * 26
        for char in s1:
            count1[ord(char) - 97] += 1

        left = 0

        for right in range(len(s2)):
            count2[ord(s2[right]) - 97] += 1
            if count1 == count2: return True
            if (right - left + 1) == len(s1):
                count2[ord(s2[left]) - 97] -= 1
                left += 1
        return False
```

