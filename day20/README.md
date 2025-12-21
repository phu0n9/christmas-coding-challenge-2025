# Day 20

### [Problem 1899: Merge Triplets to Form Target Triplet](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/description/)
Hints:
- the trick is to know the each triplet == target in the same index
- if not then continue
- use 3 variables to know which variable is True when the triplet == target in the same index

```
def mergeTriplets(self, triplets: List[List[int]], target: List[int]) -> bool:
    a = b = c = False
    for x, y, z in triplets:
        if x > target[0] or y > target[1] or z > target[2]:
            continue
        
        if x == target[0]:
            a = True
        if y == target[1]:
            b = True
        if z == target[2]:
            c = True
    
    return a and b and c
```

### [Problem 763: Partition Labels](https://leetcode.com/problems/partition-labels/description/)
Hints:
- we store the last index a character appears in a hash map
- then we have 2 start and end index, where start is the begin of the partition and end is maximum of a current character and we also have a current index to tranverse through the string
- stop when current index i == end --> append the size to the result and then update start index

```
def partitionLabels(self, s: str) -> List[int]:
    last = defaultdict(int)
    for i in range(len(s)):
        last[s[i]] = i
    start, end = 0, 0
    res = []

    for i in range(len(s)):
        end = max(end, last[s[i]])
        if i == end:
            res.append(end - start + 1)
            start = i + 1
    return res
```

### [Problem 678: Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string/description/)
Hints:
- balance = number of open parenthesis - number of close parenthesis --> if it is 0 then it is valid
- so we have 2 variables for the range of open and close parenthesis:
    - if current char == "(" --> we increment both variables
    - if current char == ")" --> we decrement both variables
    - if current char == "*" --> we can have open parenthesis or close one so we increment the open variable and decrement the close variable
    - if the open variable is < 0 --> meaning there are more close parenthesis than open one --> we return False
    - if the close variable is < 0 --> it means the char * cannot be close parenthesis so we reset it to 0
- return if the close variable is equal to 0 or not

```
def checkValidString(self, s: str) -> bool:
    min_balance = 0
    max_balance = 0
    for c in s:
        if c == '(':
            min_balance += 1
            max_balance += 1
        elif c == ')':
            min_balance -= 1
            max_balance -= 1
        else:
            min_balance -= 1
            max_balance += 1
        
        if max_balance < 0:
            return False
        min_balance = max(min_balance, 0)
    return min_balance == 0
```