# Day 1

- 3 Sliding windows 

## Sliding Windows:
### [Problem 567: Permutation in String](https://leetcode.com/problems/permutation-in-string/description/)

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

## [Problem 76: Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/)

Hints:
- store character of t in one dictionary count_t
- need = len(count_t)
- use 2 pointers, left and right where right is the current index
- use window to add character --> increment have by 1
- while have == need:
    - so if the right - left + 1 < len of t: current length = right - left + 1 and then store current left and right in an array with value of 2
    - decrement window[current char of left] by 1
    - if the window[current char of left] < count_t[current char of left] --> means it is lacking the right characters --> we decrement have by 1
    - increment left by 1
- return if res length is not default value then return "" but if it is assigned a different value then use the substring[left: right + 1]

```
def minWindow(self, s: str, t: str) -> str:
    if t == "": return ""

    count_t = defaultdict(int)
    window = defaultdict(int)

    for char in t:
        count_t[char] += 1

    have = 0
    need = len(count_t)
    res = [-1, -1]
    left = 0
    res_length = float('inf')

    for right in range(len(s)):
        c = s[right]
        window[c] += 1

        if count_t[c] == window[c]:
            have += 1
        
        while have == need:
            if right - left + 1 < res_length:
                res = [left, right]
                res_length = right - left + 1
            
            window[s[left]] -= 1

            if window[s[left]] < count_t[s[left]]:
                have -= 1
            left += 1
    return s[res[0]: res[1]+ 1] if res_length != float('inf') else ""
```

## [Problem 239: Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/description/)

Hints:
- Use deque for this
- left and right pointer where right is the current index
- use another loop inside the increment right index to keep the deque to be monotoic decreasing
- store index into deque
- if left index > first element of the queue (first index of the queue in the window) then we should pop this out
- if right + 1 >= k --> means it is outside of the window, then add the result and then move the left index by 1

```
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
    dq = collections.deque()
    res = []
    l = r = 0

    while r < len(nums):
        while dq and nums[dq[-1]] < nums[r]:
            dq.pop()
        
        dq.append(r)

        if l > dq[0]:
            dq.popleft()
        
        if r + 1 >= k:
            res.append(nums[dq[0]])
            l += 1
        r += 1
    return res
```

