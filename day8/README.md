# Day 8

- 5 Backtracking problems

### [Problem 22: Generate Parentheses](https://leetcode.com/problems/generate-parentheses/description/)
Hints:
- Stop at open == close == n
- if condition open < n --> we can add it and do recursive then pop the last parenthesis after
- second if conidition open < close --> we can add close parenthesis and do recursive and then pop it out

```
def generateParenthesis(self, n: int) -> List[str]:
    res = []
    stack = []
    def backtracking(open, close):
        if close == open == n:
            res.append(''.join(stack))
            return
        
        if open < n:
            stack.append("(")
            backtracking(open + 1, close)
            stack.pop()
        
        if close < open:
            stack.append(")")
            backtracking(open, close + 1)
            stack.pop()
    backtracking(0, 0)
    return res
```

### [Proble 79: Word Search](https://leetcode.com/problems/word-search/description/)
Hints:
- use dfs to recursive
- rememeber to set visited board[i][j] to something and then after the recursive, set it back

```
def exist(self, board: List[List[str]], word: str) -> bool:
    m = len(board)
    n = len(board[0])

    def dfs(i, j, curr):
        if i < 0 or j < 0 or i >= m or j >= n or board[i][j] == '#' or board[i][j] != word[curr]:
            return False
        
        if curr == len(word) - 1:
            return True
        
        c = board[i][j]
        board[i][j] = '#'

        found = (
            dfs(i + 1, j, curr + 1) or
            dfs(i - 1, j, curr + 1) or
            dfs(i, j + 1, curr + 1) or
            dfs(i, j - 1, curr + 1)
        )

        board[i][j] = c

        return found
    
    for i in range(m):
        for j in range(n):
            if dfs(i, j, 0):
                return True
    
    return False
```

### [Problem 131: Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/description/)
Hints:
- create function for checking palindrome
- use backtracking for it where:
    - if base condition: current iterative index == len(word) --> add it to global array
    - do a for loop with current iterative index and end with len of word:
        - with if condition with checking palindrome, if true then add it to iterative arr with s[start: end+1]
            - do backtracking 
            - pop the iterative arr

```
def partition(self, s: str) -> List[List[str]]:
    res = []
    n = len(s)
    def is_palindrome(left, right):
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        return True
    
    def backtrack(start, arr):
        if start == n:
            res.append([*arr])              
            return
        
        for end in range(start, n):
            if is_palindrome(start, end):
                arr.append(s[start:end + 1])
                backtrack(end + 1, arr)
                arr.pop()
    
    backtrack(0, [])

    return res
```

### [Problem 17: Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
Hints:
- Create hashmap of number and characters
- use backtrack where :
    - if base condition is current index == len word then add it to global result
    - for loop through the array of characters of current index then add current character to iterative arr, backtrack to the next current index and then pop out in the iterative arr the last character

```
def letterCombinations(self, digits: str) -> List[str]:
    if not digits:
        return []
    
    digit_map = {
        '2': ['a', 'b', 'c'],
        '3': ['d', 'e', 'f'],
        '4': ['g', 'h', 'i'],
        '5': ['j', 'k', 'l'],
        '6': ['m', 'n', 'o'],
        '7': ['p', 'q', 'r', 's'],
        '8': ['t', 'u', 'v'],
        '9': ['w', 'x', 'y', 'z']
    }
    res = []
    n = len(digits)

    def backtrack(start, arr):
        if start == n:
            res.append("".join(arr))
            return
        
        for ch in digit_map[digits[start]]:
            arr.append(ch)
            backtrack(start + 1, arr)
            arr.pop()

    backtrack(0, [])

    return res
```

### [Problem 51: N-Queens](https://leetcode.com/problems/n-queens/description/)
Hints:
- The trick is we cannot allow 2 queens with the same column or diagnol 
- And there are two kinds of diagnol (right to left or left to right)
- However, in the same diagnol line from left to right, all the position can be the same as (r - c)
- in the same diagnol line from right to left, all the position can be the same as (r + c)
- So we need to keep track of the position of these using 3 different hashset: column, positive_diagnol (right to left), negative_diagnol (left to right)
- So we need to iterate through each row, and keep track of column, and 2 direction diagnol positions
- Remember to set the current board[r][c] to 'Q' and set it back after we finished the recursive as well as remove the current position of col or 2 diagnols

```
def solveNQueens(self, n: int) -> List[List[str]]:
    res = []
    board = [["."] * n for row in range(n)]
    col = set()
    pos_diagonal = set() # r + c
    neg_diagonal = set() # r - c

    def backtrack(r):
        if r == n:
            copy = ["".join(row) for row in board]
            res.append(copy)
            return
        
        for c in range(n):
            if c in col or (r + c) in pos_diagonal or (r - c) in neg_diagonal:
                continue
            
            board[r][c] = 'Q'
            col.add(c)
            pos_diagonal.add(r + c)
            neg_diagonal.add(r - c)

            backtrack(r + 1)

            board[r][c] = '.'
            col.remove(c)
            pos_diagonal.remove(r + c)
            neg_diagonal.remove(r - c)

    backtrack(0)
    return res
```