# Day 9

### [Problem 200: Number of Islands](https://leetcode.com/problems/number-of-islands/description/)
Hints:
- use dfs and no need to revisit the positions based on setting to '0'
- only use dfs when it is current grid position is '1'

```
def numIslands(self, grid: List[List[str]]) -> int:
    m, n = len(grid), len(grid[0])
    res = 0

    def dfs(i, j):
        if i < 0 or j < 0 or i >= m or j >= n or grid[i][j] == '0':
            return
        
        grid[i][j] = '0'
        dfs(i + 1, j)
        dfs(i - 1, j)
        dfs(i, j + 1)
        dfs(i, j - 1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                dfs(i, j)
                res += 1
    
    return res
```

### [Problem 695: Max Area of Island](https://leetcode.com/problems/max-area-of-island/description/)
Hints:
- The same if base condition like numOfIslands and set it to grid[i][j] = 0 to know we have visited there
- with every problem that require to find the area or sum or anything, just return like 1 + dfs(...)

```
def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
    max_area = 0
    m, n = len(grid), len(grid[0])

    def dfs(i, j):
        if i < 0 or j < 0 or i >= m or j >= n or grid[i][j] == 0:
            return 0
        
        grid[i][j] = 0
        return 1 + dfs(i + 1, j) + dfs(i - 1, j) + dfs(i, j + 1) + dfs(i, j - 1)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == 1:
                max_area = max(max_area, dfs(i, j))
    
    return max_area
```
