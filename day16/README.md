# Day 16

### [Problem 846: Hand of Straights](https://leetcode.com/problems/hand-of-straights/description/)
Hints:
- Check if the number is divisible for groupSize
- use freq map and iterate it with index of group size like 1, 2, 3 are good for group size 3 but if some of the increasing number is missing then return false
- also need to use min heap to start from smallest number and remember to pop out the numbers in heap that are not existing in freq map

```
def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
    if len(hand) % groupSize != 0: return False

    count = Counter(hand)

    heap = hand
    heapq.heapify(heap)

    while count:
        while heap[0] not in count:
            heapq.heappop(heap)
        
        start = heapq.heappop(heap)
        for idx in range(groupSize):
            if start + idx not in count:
                return False
            
            count[start + idx] -= 1

            if count[start + idx] == 0:
                del count[start+idx]
    return True
```

### [Problem 62: Unique Paths](https://leetcode.com/problems/unique-paths/description/)
Hints:
- Use dp
- This only do right or down so the first row would only have 1 way for each cell and first column as well
- set the first row and first column is 1 and start 2d array at 1:
    - cell[i][j] = cell[i-1][j] (right) + cell[i][j-1] (bottom)
- return the bottom right cell as answer

```
def uniquePaths(self, m: int, n: int) -> int:
    board = [[1] * n for _ in range(m)]
    
    for i in range(1, m):
        for j in range(1, n):
            board[i][j] = board[i-1][j] + board[i][j-1]
    
    return board[m-1][n-1]
```

### [Problem 309: Best Time to Buy and Sell Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)
Hints:
- use dp for it
- when it comes a decision, either:
    - you buy or cooldown 
    - sell and res or buy again
- so we have 3 options:
    - buy -> hold -> sell --> cooldown
    - buy -> sell --> cooldown
    - sell --> cooldown -> buy
- If we sell then we have to wait 2 days
- We have to find the max between buy or hold and max between sell or hold

```
def maxProfit(self, prices: List[int]) -> int:
    memo = {}
    def dfs(day, can_buy):
        if day >= len(prices):
            return 0
        
        if (day, can_buy) in memo:
            return memo[(day, can_buy)]
        
        skip = dfs(day + 1, can_buy)
        if can_buy:
            buy = dfs(day + 1, False) - prices[day]
            memo[(day, can_buy)] = max(buy, skip)
        else:
            sell = dfs(day + 2, True) + prices[day]
            memo[(day, can_buy)] = max(sell, skip)
        return memo[(day, can_buy)]
    return dfs(0, True)
```

### [Problem 45: Jump Game II](https://leetcode.com/problems/jump-game-ii/description/)
Hints:
- It looks like bfs without a queue --> greedy approach but can be solved by dynamic programming (O n^2)
- use 2 pointers left and right
- where left is the current index and right is the farthest it can go
- in a nested for loop to find the farthest from left and right + 1 --> max(farthest, current index in window of left and right + nums[current index])
- outside nested loop after finding farthest: update left = right + 1 and right = farthest
- increment result to 1 after the jump

```
def jump(self, nums: List[int]) -> int:
    res = 0
    l, r = 0, 0
    while r < len(nums) - 1:
        farthest = 0
        for i in range(l, r + 1):
            farthest = max(farthest, i + nums[i])
        l = r + 1
        r = farthest
        res += 1
    return res
```
