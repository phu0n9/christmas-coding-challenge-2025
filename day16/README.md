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